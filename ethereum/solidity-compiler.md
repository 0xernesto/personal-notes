---
description: Guide to managing Solidity compiler versions with svm-rs.
---

# Solidity Compiler

## Solidity Compiler Management

Alloy's [svm-rs](https://github.com/alloy-rs/svm-rs) is the easiest way to manage Solidity compiler versions on your machine. This is the version manager that [Foundry](https://github.com/foundry-rs/foundry) uses, so if you already have Foundry installed, you can skip the installation steps.

### Installation

1. Install Rust (if not already installed):

```
curl https://sh.rustup.rs -sSf | sh
```

2. Install the [svm-rs](https://github.com/alloy-rs/svm-rs) crate:

```
cargo install svm-rs
```

### Usage

List installed and available versions:

```
svm list
```

Install a version:

```
svm install <VERSION>
```

Set global version from installed versions:&#x20;

```
svm use <VERSION>
```

Remove an installed version:&#x20;

```
svm remove <VERSION>
```

List exact `solc` commit (replace `x` and `y` with your version numbers):

```
~/.svm/0.x.y/solc-0.x.y --version
```

or you can view the exact `solc` commit used by your Foundry project by going to `out/YourContract.sol/YourContract.json` and looking at `metadata.compiler.version`, as shown below:

<figure><img src="../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

Knowing the exact version is important verifying your deployed contract on a block explorer, like Etherscan.
