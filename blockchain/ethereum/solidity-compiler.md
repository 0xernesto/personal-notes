---
description: Guide to managing Solidity compiler versions with svm-rs.
---

# Solidity Compiler

## <mark style="color:purple;">Solidity Compiler Management</mark>

Alloy's [svm-rs](https://github.com/alloy-rs/svm-rs) is the easiest way to manage Solidity compiler versions on your machine. This is the version manager that [Foundry](https://github.com/foundry-rs/foundry) uses, so if you already have Foundry installed, you can skip the installation steps.

### Installation

1. Install Rust (if not already installed):

```markup
curl https://sh.rustup.rs -sSf | sh
```

2. Install the [svm-rs](https://github.com/alloy-rs/svm-rs) crate:

```markup
cargo install svm-rs
```

### Usage

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
