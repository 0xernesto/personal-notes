---
description: >-
  Smart contract development toolkit for managing dependencies, compiling,
  testing, deploying, and interacting with smart contracts from the command line
  and via Solidity scripts.
---

# Foundry

## <mark style="color:purple;">Overview</mark>

Foundry is an alternative to tools like Hardhat, Truffle, Brownie, etc.

There are two main flaws with tools like Hardhat, Truffle, and Brownie:

1. They require the use of another language like JavaScript or Python for writing scripts.
2. They are slow.

Foundry is the superior choice is because:

1. Scripts can be written Solidity, the same language we use to write Ethereum smart contracts.
2. We can trigger actions and interact with deployed contracts directly from the command line, which is a very nice bonus.
3. Foundry is incredibly fast because it was written in Rust.

### Official Links

<img src="../../.gitbook/assets/foundry.png" alt="" data-size="line"> [Foundry Book (Docs)](https://book.getfoundry.sh/)

<img src="../../.gitbook/assets/GitHub-Mark-Light-32px.png" alt="" data-size="line"> [Foundry Github](https://github.com/foundry-rs/foundry)

## <mark style="color:purple;">Manage Solidity Compiler (solc)</mark>

Foundry relies on `svm` to install native builds for Apple Silicon machines. All `solc` versions are installed under `~/.svm/`.

Here's how to set up `solc` version management with `svm` on your Apple Silicon machine:

1. Install Rust:

```markup
curl https://sh.rustup.rs -sSf | sh
```

2. Install [svm-rs](https://github.com/roynalnaruto/svm-rs):

```markup
cargo install svm-rs
```

That's it. Here are some usage examples:

List installed and available versions:

```
svm list
```

Install a version:

```markup
svm install <VERSION>
```

Set global version from installed versions:&#x20;

```markup
svm use <VERSION>
```

Remove an installed version:&#x20;

```markup
svm remove <VERSION>
```

List exact `solc` commit (replace x and y with your version numbers):

```markup
~/.svm/0.x.y/solc-0.x.y --version
```

## <mark style="color:purple;">Forge Commands</mark>

The `forge` command is used to build, test, and deploy smart contracts.

### Compile a Contract

* [Foundry Book - forge build](https://book.getfoundry.sh/reference/forge/forge-build)
* The following command will compile all contracts inside the `/src` directory and save the output in the `/out` directory.

```markup
forge build
```

### Deploy a Contract

* [Foundry Book - forge create](https://book.getfoundry.sh/reference/forge/forge-create)
* The `--etherscan-api-key <API_KEY> --verify` part is only required if you would like your contract to be automatically [verified](https://etherscan.io/verifyContract) on the chain's block explorer, which is highly recommended. You can get an API key by creating an account on the chain's block explorer (e.g., Etherscan, Arbiscan, Polygonscan, etc.).

{% code overflow="wrap" %}
```markup
forge create <CONTRACT_FILE_PATH>:<CONTRACT_NAME> --constructor-args <ARG1> <ARG2> --rpc-url <RPC_URL> --private-key <PRIVATE_KEY> --etherscan-api-key <API_KEY> --verify
```
{% endcode %}

### Run a Specific Unit Test

* [Foundry Book - forge test](https://book.getfoundry.sh/forge/tests)
* The following command assumes that the unit test will be running on a forked network.
* The `-vvvvv` flag specifies the level of verbosity (i.e., how much detail to display in the terminal as the test runs). In this case, we are specifying the highest level (Level 5).\
  \
  If we don't specify this flag, Level 1 is assumed, `-vv` = Level 2, `-vvv` = Level 3, etc.

{% code overflow="wrap" %}
```markup
forge test --fork-url <RPC_URL> --match-test <TEST_NAME> -vvvvv
```
{% endcode %}

### Run All Unit Tests

* [Foundry Book - forge test](https://book.getfoundry.sh/forge/tests)
* The following command assumes that the unit test will be running on a forked network.
* Also, the minimum amount of detail will be displayed in the terminal as the tests run, since the default verbosity is Level 1.

{% code overflow="wrap" %}
```markup
forge test --fork-url <RPC_URL>
```
{% endcode %}

## <mark style="color:purple;">Cast Commands</mark>

The `cast` command is used to perform Ethereum RPC calls to deployed smart contracts.

### Call a Function

* [Foundry Book - cast call](https://book.getfoundry.sh/reference/cast/cast-call)
* Calling a contract function **does not modify any on-chain data**. This is simply a way to read data returned by the function that we call using the `cast call` command.

{% code overflow="wrap" %}
```markup
cast call <CONTRACT_ADDRESS> "functionName(<PARAM1_TYPE>, <PARAM2_TYPE>)" <PARAM1> <PARAM2> --rpc-url <RPC_URL>
```
{% endcode %}

### Submit  a Transaction

* [Foundry Book - cast send](https://book.getfoundry.sh/reference/cast/cast-send)
* Submitting a transaction to a contract function **will modify on-chain data.** In order to successfully do this, we need to sign the transaction with our private key and pay the corresponding gas fee.

{% code overflow="wrap" %}
```markup
cast send <CONTRACT_ADDRESS> "functionName(<PARAM1_TYPE>, <PARAM2_TYPE>)" <PARAM1> <PARAM2> --rpc-url <RPC_URL> --private-key <PRIVATE_KEY>
```
{% endcode %}

### Send Native Token to Account

The amount can be an integer, which defaults to Wei, or you can specify the units. For example `1ether`.&#x20;

```markup
cast send <SOME_ADDRESS> --value <AMOUNT> --private-key <KEY>
```

## <mark style="color:purple;">Testing on Local Mainnet Fork</mark>

### Create a Local Fork

The `anvil` command is used to create a local Mainnet fork for deploying and testing smart contracts.

[Foundry Book - anvil reference](https://book.getfoundry.sh/reference/anvil/)

* The `--chain-id` and `--port` commands are optional.
* If `--chain-id` is not specified, the default value is set to it's Mainnet counterpart (i.e., chain ID would be 1 for Ethereum).
* If `--port` is not specified, the default value is set to 8545.

```markup
anvil --fork-url <RPC_URL> --chain-id <CHAIN_ID> --port <PORT>
```

### Impersonate an Account

This allows you to submit transactions as if you were the owner of the account you are impersonating.

```markup
cast rpc anvil_impersonateAccount <SOME_ADDRESS>
```
