---
description: Overview for Developing Gas-Efficient Smart Contracts
---

# Gas Optimization

**NOTE:** This overview is a formalization of my notes from one of [Jeffrey Scholtz's ](https://twitter.com/jeyffre)courses. I highly recommend taking any of his [Udemy courses](https://www.udemy.com/user/jeffrey-scholz/) or [RareSkills bootcamps](https://www.rareskills.io/web3-blockchain-bootcamps).

## <mark style="color:purple;">Introduction</mark>

To master Ethereum development, there are three areas that one should be very competent in:

1. Design Patterns
2. Security
3. Gas Optimization
   * This will be the primary focus of this overview.

It is important for smart contract engineers to be conscious of gas optimization, as our design choices impact not only our users, but the entire Ethereum network. This overview is meant to aid in grasping the broader implications of these choices.

## <mark style="color:purple;">Gas Basics</mark>

The gas cost of a transaction depends on exactly 5 factors:

1. The transaction data that was sent.
2. The amount of memory that was used.
3. The state changes that took place.
4. The opcodes that were executed.
5. The current gas price.

The gas units of a transaction are static and determined by 1-4 above. We can think of gas units as measured units of computation.

The gas price varies based on network congestion. If a lot of people are using the network, you'll have to pay more to get you're transaction processed by a validator. Real-time gas prices can be obtained from any [gas tracker](https://etherscan.io/gastracker) online.

To calculate the gas cost of a transaction, we simply multiply its gas units by the gas price.

$$
GasCost_{gwei}=GasUnits*GasPrice_{gwei}
$$

$$
GasCost_{usd}=\frac{GasUnits*GasPrice_{gwei}*EthPrice_{usd}}{10^9}
$$

Gas optimization focuses on taking advantage of controllable factors to reduce the cost of transactions. For example, by making better design decisions, we can reduce the gas units of a particular on-chain action by 50%, but we can't control the gas prices or the price of ETH.

The cheapest transaction we can submit to the Ethereum network is a native transfer (e.g., Alice transfers 5 ETH to Bob), which costs 21,000 gas units. This means that <mark style="color:yellow;">all transactions on Ethereum must cost at least 21,000 gas units.</mark>

## <mark style="color:purple;">Block Limit</mark>

The block limit is simply the maximum block size. For Bitcoin, the block limit is 1 MB. Ethereum, does not have an explicit byte limit. Instead, Ethereum limits the amount of computations per block, which means the Ethereum block limit is defined in gas units. <mark style="color:yellow;">The current Ethereum block limit is 30 million gas units.</mark>

Theoretically, a block limit of 30 million gas units can fit 1,428 native transfer transactions, since each costs 21,000 gas units. On the other extreme, this limit can only fit 30 tornado cash transactions, since each costs \~1 million gas units.

### Throughput

The throughput of the network is defined as the number of transactions per second (TPS) that can be verified.

<mark style="color:yellow;">A new Ethereum block is generated every 15 seconds.</mark> This means that if all transactions were native transfers, the throughput would be 95 TPS.

$$
\frac{1428 \; TXs}{1 \; Block}*\frac{1 \; Block}{15 \; seconds} = 95 \; TPS
$$

Similarly, if all transactions were tornado cash transactions, the throughput would be 2 TPS.

At time of writing this, the actual Ethereum throughput ranges from 15-25 TPS.

### Implications

* There cannot be more than 1428 transactions in one block.
* The max throughput for Ethereum is 95 TPS.
* The highest bidders will get their transactions included in the block. This is why gas prices fluctuate.
* One native transfer transaction (21,000 gas units) is 0.07% of Ethereum's computational capacity per 15 seconds.
* If a transaction requires more than 30 million gas units to execute, it will never execute because it does not fit in a single block.
* <mark style="color:yellow;">Your design choices as a smart contract engineer impact not only your users but the entire Ethereum network.</mark>

## <mark style="color:purple;">Storage</mark>

Ethereum smart contracts employ a storage model that allows for persistent storage of state variables in the [EVM](../../glossary.md). This model consists of two important concepts, storage layout and storage slots. Understanding these is key for gas optimization and data integrity in smart contracts, as mismanagement can result in vulnerabilities and high gas costs. Below are two brief explanations of storage layout and slots, but further [reading](https://docs.alchemy.com/docs/smart-contract-storage-layout) is highly encouraged.

### Storage Layout

This refers to the way data is organized and accessed within a smart contract. Smart contracts on Ethereum have a key-value storage model, where each key is a 256-bit number, and each value is also 256 bits. The storage layout dictates how different variables (like integers, addresses, or more complex data structures) are mapped to these 256-bit keys.

### Storage Slots

Each 256-bit key in the key-value storage model is referred to as a "slot". Every slot can store a value up to 256 bits. The values stored in these slots correspond to the state variables defined in the smart contract. <mark style="color:yellow;">The order of state variable declarations and the type of each variable impacts storage efficiency and the cost of reading from and writing to these variables.</mark>

### Accessing Slots in Solidity

<mark style="color:yellow;">The EVM uses storage slots to know which values to access. In other words, the EVM does not care what we name our variables because it only understands the location of our variables.</mark>

To better understand the nature of storage slots, let's use some code. The smart contract below contains one state variable, `a`, and a function that returns the storage slot of `a`.

Note that we use an [inline assembly (Yul)](https://docs.soliditylang.org/en/latest/yul.html) block to fetch the storage slot of variable `a`.

```solidity
contract Storage {
    uint256 private a;
    
    function storageLocation() external pure returns(uint256) {
        uint256 slotLocation;
        
        assembly {
            slotLocation := a.slot
        }
        
        return slotLocation:
    }
}
```

If we called `storageLocation()`, it would return `0`. If we declared a variable before `a`, then `storageLocation()` would return `1`.

Now, if we wanted to fetch the value of a variable given a storage slot, we can refactor our smart contract as shown below.

```solidity
contract Storage {
    uint256 private a = 99;
    uint256 private b = 13;
    uint256 private c = 2;
    
    function getSlotValue(uint256 slot) external view returns(uint256) {
        uint256 value;
        
        assembly {
            value := sload(slot)
        }
        
        return value:
    }
}
```

Calling `getSlotValue(0)` would return `99`,  `getSlotValue(1)` would return `13`, and `getSlotValue(2)` would return `2`.

This roughly demonstrates how the order of variable declarations in a smart contract impacts the location of their storage slots. To expand on this a bit further, if we declared a combination of different variable types, such as `uint256`, `uint8`, `bool`, and `address[]`, it's important to be more cautious with how we declare our storage variables. For example, we could pack the `uint8` and `bool` variables in the same slot. However, packing these variables together is only advantageous if these two variables are always used in conjunction. If they are not, the storage gas savings does not outweigh the increased cost of reading from or writing to only one of the two variables in the slot.

Lastly, it's crucial to remember that once a smart contract is deployed, the storage slots for its variables are fixed. This is especially important in contracts that use a [proxy pattern](https://docs.openzeppelin.com/upgrades-plugins/1.x/proxies), where meticulous management of storage slots is vital to avoid storage collisions.

For a more in-depth understanding of storage, checkout [Alchemy's storage layout guide.](https://docs.alchemy.com/docs/smart-contract-storage-layout)

## <mark style="color:purple;">Opcodes</mark>

To understand what opcodes are, let's consider the smart contract below. From a semantic perspective, the language of the Solidity code is very straight forward. However, this language is extremely foreign to a computer. <mark style="color:yellow;">Since the EVM is a computer, we need to translate the Solidity code to a language that the EVM can understand by compiling the Solidity code. The EVM understands assembly code, which is a set of highly specific and concise instructions designed to carry out specific tasks. These instructions are called opcodes.</mark>

**NOTE:** The EVM is a stack-based machine. This means that a [stack](https://en.wikipedia.org/wiki/Stack\_\(abstract\_data\_type\)) is the primary data structure used for handling low-level instructions (opcodes) in a sequential, [LIFO](../../glossary.md) order.

```solidity
contract Test {
    uint256 private a = 3;
    
    function aPlusOne() external view returns(uint256) {
        return a + 1;
    }
}
```

Given the code above, the following is a simplified breakdown of the most relevant opcodes that will execute on the EVM when `aPlusOne()` is called.

<table><thead><tr><th width="136">Opcode</th><th width="254">Description</th><th width="125">Stack State</th></tr></thead><tbody><tr><td><code>PUSH1 00</code></td><td>Push <code>0</code> onto the stack.</td><td>0</td></tr><tr><td><code>SLOAD</code></td><td>Treat <code>0</code> as a storage slot and load the corresponding value.</td><td>3</td></tr><tr><td><code>PUSH1 01</code></td><td>Push <code>1</code> onto the stack.</td><td>1<br>3</td></tr><tr><td><code>ADD</code></td><td>Add <code>1</code> and <code>3</code>.</td><td>4</td></tr></tbody></table>

The state of the stack is especially important to pay attention to. In this example, only the two values of interest are present on the stack when performing the ADD operation. In cases where we have more than two values on the stack, additional opcodes may be necessary to move the values of interest to the top of the stack.

Each EVM opcode has a specific cost in terms of gas units. Some are more expensive than others, depending on the complexity of the operation. The majority of the gas cost associated with executing a transaction is simply the sum of all the opcodes executed within that transaction.

For a full, comprehensive list of opcodes, here are two resources I have found helpful:

* [wolflo/evm-opcodes](https://github.com/wolflo/evm-opcodes)
* [evm.codes (interactive)](https://www.evm.codes/)

