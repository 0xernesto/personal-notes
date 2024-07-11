---
description: Overview for Developing Gas-Efficient Smart Contracts
---

# Gas Optimization

**NOTE:** This overview is a formalization of my notes from one of [Jeffrey Scholtz's ](https://twitter.com/jeyffre)courses. I highly recommend taking any of his [Udemy courses](https://www.udemy.com/user/jeffrey-scholz/) or [RareSkills bootcamps](https://www.rareskills.io/web3-blockchain-bootcamps).

## Introduction

To master Ethereum development, there are three areas that one should be very competent in:

1. Design Patterns
2. Security
3. Gas Optimization
   * This will be the primary focus of this overview.

It is important for smart contract engineers to be conscious of gas optimization, as our design choices impact not only our users, but the entire Ethereum network. This overview is meant to aid in grasping the broader implications of these choices.

## Gas Basics

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

The cheapest transaction we can submit to the Ethereum network is a native transfer (e.g., Alice transfers 5 ETH to Bob), which costs 21,000 gas units. This means that all transactions on Ethereum must cost at least 21,000 gas units.

## Block Limit

The block limit is simply the maximum block size. For Bitcoin, the block limit is 1 MB. Ethereum, does not have an explicit byte limit. Instead, Ethereum limits the amount of computations per block, which means the Ethereum block limit is defined in gas units. The current Ethereum block limit is 30 million gas units.

Theoretically, a block limit of 30 million gas units can fit 1,428 native transfer transactions, since each costs 21,000 gas units. On the other extreme, this limit can only fit 30 tornado cash transactions, since each costs \~1 million gas units.

### Throughput

The throughput of the network is defined as the number of transactions per second (TPS) that can be verified.

A new Ethereum block is generated every 15 seconds. This means that if all transactions were native transfers, the throughput would be 95 TPS.

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
* Your design choices as a smart contract engineer impact not only your users but the entire Ethereum network.

## Storage

Ethereum smart contracts employ a storage model that allows for persistent storage of state variables in the [EVM](../glossary.md). This model consists of two important concepts, storage layout and storage slots. Understanding these is key for gas optimization and data integrity in smart contracts, as mismanagement can result in vulnerabilities and high gas costs. Below are two brief explanations of storage layout and slots, but further [reading](https://docs.alchemy.com/docs/smart-contract-storage-layout) is highly encouraged.

### Storage Layout

This refers to the way data is organized and accessed within a smart contract. Smart contracts on Ethereum have a key-value storage model, where each key is a 256-bit number, and each value is also 256 bits. The storage layout dictates how different variables (like integers, addresses, or more complex data structures) are mapped to these 256-bit keys.

### Storage Slots

Each 256-bit key in the key-value storage model is referred to as a "slot". Every slot can store a value up to 256 bits. The values stored in these slots correspond to the state variables defined in the smart contract. The order of state variable declarations and the type of each variable impacts storage efficiency and the cost of reading from and writing to these variables.

### Accessing Slots in Solidity

The EVM uses storage slots to know which values to access. In other words, the EVM does not care what we name our variables because it only understands the location of our variables.

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

## Opcode Basics

To understand what opcodes are, let's consider the smart contracts below. From a semantic perspective, the language of the Solidity code is very straight forward. However, this language is extremely foreign to a computer. Since the EVM is a computer, we need to translate the Solidity code to a language that the EVM can understand by compiling the Solidity code. The EVM understands assembly code, which is a set of highly specific and concise instructions designed to carry out specific tasks. These instructions are called opcodes.

**NOTE:** The EVM is a stack-based machine. This means that a [stack](https://en.wikipedia.org/wiki/Stack\_\(abstract\_data\_type\)) is the primary data structure used for handling low-level instructions (opcodes) in a sequential, [LIFO](../glossary.md) order.

```solidity
contract TestContractOne {
    uint256 private a = 3;
    
    function doTheThing() external view returns(uint256) {
        return a + 1;
    }
}
```

Given the code above, the following is a simplified breakdown of the most relevant opcodes that will execute on the EVM when `doTheThing()` is called.

<table><thead><tr><th width="149">Opcode</th><th width="257">Description</th><th>Stack State</th></tr></thead><tbody><tr><td><code>PUSH1 00</code></td><td>Push <code>0</code> onto the stack.</td><td>0</td></tr><tr><td><code>SLOAD</code></td><td>Treat <code>0</code> as a storage slot and load the corresponding value.</td><td>3</td></tr><tr><td><code>PUSH1 01</code></td><td>Push <code>1</code> onto the stack.</td><td>3<br>1 &#x3C;- top of stack</td></tr><tr><td><code>ADD</code></td><td>Add <code>1</code> and <code>3</code>.</td><td>4</td></tr></tbody></table>

Here is a slightly more complex example:

```solidity
contract TestContracTwo {
    uint256 private a = 3;
    uint256 private b = 6;
    
    function doTheThing() external view returns(uint256) {
        return 5 * a + 4 * b;
    }
}
```

The simplified opcode breakdown looks like this:

<table><thead><tr><th width="149">Opcode</th><th width="257">Description</th><th>Stack State</th></tr></thead><tbody><tr><td><code>PUSH1 05</code></td><td>Push <code>5</code> onto the stack.</td><td>5</td></tr><tr><td><code>PUSH1 00</code></td><td>Push <code>0</code> onto the stack.</td><td>5<br>0 &#x3C;- top of stack</td></tr><tr><td><code>SLOAD</code></td><td>Treat <code>0</code> as a storage slot and load the corresponding value.</td><td><p>5</p><p>3 &#x3C;- top of stack</p></td></tr><tr><td><code>PUSH1 04</code></td><td>Push <code>4</code> onto the stack.</td><td><p>5</p><p>3</p><p>4 &#x3C;- top of stack</p></td></tr><tr><td><code>PUSH1 01</code></td><td>Push <code>1</code> onto the stack.</td><td><p>5</p><p>3</p><p>4</p><p>1 &#x3C;- top of stack</p></td></tr><tr><td><code>SLOAD</code></td><td>Treat <code>1</code> as a storage slot and load the corresponding value.</td><td><p>5</p><p>3</p><p>4</p><p>6 &#x3C;- top of stack</p></td></tr><tr><td><code>MUL</code></td><td>Multiply <code>6</code> and <code>4</code>. </td><td><p>5</p><p>3</p><p>24 &#x3C;- top of stack</p></td></tr><tr><td><code>SWAP2</code></td><td>Swap the topmost value in the stack with the value located two positions below it.</td><td><p>24</p><p>3</p><p>5 &#x3C;- top of stack</p></td></tr><tr><td><code>MUL</code></td><td>Multiply <code>5</code> and <code>3</code>. </td><td><p>24</p><p>15 &#x3C;- top of stack</p></td></tr><tr><td><code>ADD</code></td><td>Add <code>15</code> and <code>24</code>.</td><td>39</td></tr></tbody></table>

Each EVM opcode has a specific cost in terms of gas units. Some are more expensive than others, depending on the complexity of the operation. The majority of the gas cost associated with executing a transaction is simply the sum of all the opcodes executed within that transaction.

For a full, comprehensive list of opcodes and their associated cost, here are two helpful resources:

* [wolflo/evm-opcodes](https://github.com/wolflo/evm-opcodes)
* [evm.codes (interactive)](https://www.evm.codes/)

## Function Selectors

To understand what a function selector is, we first need to understand what a function signature is in the context of Ethereum smart contracts.

**Function Signature**

* A concise representation of a function that includes the function's name and the types of its input parameters
* Ex: `transfer(address,uint256)`
* Ex: `someFunc(bool[],bytes)`

**Function Selector**

* The first 4 bytes of the `keccak-256` hash of the function signature, used to identify functions in bytecode.
* Ex: `keccak256("transfer(address,uint256)")` -> `0xa9059cbb`
* Ex: `keccak256("someFunc(bool[],bytes)")`  -> `0x7a2cf21d`

Now, let’s recall that there are two types of function calls that we can make to smart contracts:

1. `read`: functions that only read data without making any state changes.
2. `write`: functions that alter the contract’s state or involve ETH transfers, which requires a transaction to be published.

When calling a `write` function on a smart contract, we are simply sending a transaction to the address of the smart contract. However, for the smart contract to know which function we want to interact with, the function selector and the ABI-encoded function parameter values must be included in the `input` field of the transaction object. If the function has no parameters, only the function selector must be included. This, of course, is abstracted away from users, but it's important to understand.

For reference, here is an example of a transaction object.

```json
{
    "from": "0x1923f626bb8dc025849e00f99c25fe2b2f7fb0db",
    "gas": "0x55555",
    "maxFeePerGas": "0x1234",
    "maxPriorityFeePerGas": "0x1234",
    "input": "0xabcd",
    "nonce": "0x0",
    "to": "0x07a565b7ed7d7a678680a4c162885bedbb695fe0",
    "value": "0x1234"
 }
```

Now, let's consider the smart contract below.

```solidity
contract TestContract {
    function doNothing(uint256 someNumber) external payable {
        // do nothing
    }
}
```

Say we want to call `doNothing(uint256 someNumber)` on this smart contract and send it 100 Wei. The following two Foundry commands are equivalent ways to do accomplish this.

{% code overflow="wrap" %}
```bash
cast send 0xc5Ae07D32067005CC098240A44828Cd7A087d4FC "doNothing(uint256)" 100 --value 0.0000000000000001ether --rpc-url <RPC_URL> --private-key <PRIVATE_KEY>
```
{% endcode %}

{% code overflow="wrap" %}
```bash
cast send 0xc5Ae07D32067005CC098240A44828Cd7A087d4FC 0xdce1d5ba0000000000000000000000000000000000000000000000000000000000000064 --value 0.0000000000000001ether --rpc-url <RPC_URL> --private-key <PRIVATE_KEY>
```
{% endcode %}

Clearly, the first command is easier to understand, but the second command shows that we can get the same result by explicitly providing the function selector and ABI-encoded parameter in the transaction’s `input` field.

To verify this, the smart contract above was deployed to Gnosis, and two transactions were published with the commands above.

Deployed contract: [0xc5Ae0...7d4FC](https://gnosisscan.io/address/0xc5Ae07D32067005CC098240A44828Cd7A087d4FC)

TX sent with 1st Command: [0x2dc78...1e30f](https://gnosisscan.io/tx/0x2dc78ba747fd432f95ec2a0b9433387937b8f7a471a4c3d017a8cee44b91e30f)

TX sent with 2nd Command: [0x9ee16...7f2e1](https://gnosisscan.io/tx/0x9ee169f4ca5f470e5be11efc4ff9184ea4203406b92637b3cfdca171a097f2e1)

Both transactions resulted in the same outcome. Below are screenshots of what the input data looks like for both transactions on Gnosisscan. The first image is the default (decoded) view, while the second is the actual hex data that was included in the `input` field.

Additionally, notice that the hex data in the images below is the same as the 2nd Foundry command above.

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption><p>Decoded <code>input</code> field of transaction object on Gnosisscan.</p></figcaption></figure>

<figure><img src="../.gitbook/assets/image (1) (1).png" alt=""><figcaption><p>Original <code>input</code> field of transaction object on Gnosisscan.</p></figcaption></figure>

## More Coming Soon...

Coming soon...
