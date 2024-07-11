---
description: Guide to managing Solidity compiler versions with svm-rs.
---

# Solidity Compiler

## Solidity Compiler Management

Alloy's [svm-rs](https://github.com/alloy-rs/svm-rs) is the easiest way to manage Solidity compiler versions on your machine. This is the version manager that [Foundry](https://github.com/foundry-rs/foundry) uses, so if you already have Foundry installed, you can skip the installation steps.

### Installation

1. Install Rust (if not already installed):

```bash
curl https://sh.rustup.rs -sSf | sh
```

2. Install the [svm-rs](https://github.com/alloy-rs/svm-rs) crate:

```bash
cargo install svm-rs
```

### Usage

List installed and available versions:

```bash
svm list
```

Install a version:

```bash
svm install <VERSION>
```

Set global version from installed versions:&#x20;

```bash
svm use <VERSION>
```

Remove an installed version:&#x20;

```bash
svm remove <VERSION>
```

### Compiler Version

Knowing the exact compiler version is important for verifying deployed contracts on block explorers, like Etherscan.

To list the exact `solc` commit, run the following in your terminal (replace `x` and `y` with your version numbers):

```bash
~/.svm/0.x.y/solc-0.x.y --version
```

Alternatively, you can view the exact `solc` commit used by your Foundry project by going to `out/YourContract.sol/YourContract.json` and looking at `metadata.compiler.version`, as shown below:

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>
