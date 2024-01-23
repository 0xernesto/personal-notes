---
description: Useful ZSH Commands
---

# ZSH

<mark style="color:yellow;">**NOTE:**</mark> The following commands are largely the same as BASH commands, since they're both UNIX-based shells.

## <mark style="color:purple;">Basic Commands</mark>

### Change the Current Directory

```markup
cd <DIR_NAME>
```

### List Directory Contents

```shell
ls
```

### Move or Rename Directories

```markup
mv <SOURCE> <DESTINATION>
```

### Remove Files Directories

```markup
rm <FILE_OR_DIR_NAME>
```

### Set an Environment Variable

```markup
export VARIABLE_NAME=<SOME_VALUE>
```

### Display an Environment Variable

```bash
echo $VARIABLE_NAME
```

### Display an Entire File

```markup
cat <FILE_NAME>
```

### Display the First N Lines of a File

Fist 10 Lines:

<pre class="language-markup"><code class="lang-markup"><strong>head &#x3C;FILE_NAME>
</strong></code></pre>

First 5 Lines:

<pre class="language-markup"><code class="lang-markup"><strong>head -n 5 &#x3C;FILE_NAME>
</strong></code></pre>

### Show Command History

```bash
history
```

## <mark style="color:purple;">Clear Terminal History</mark>

1. Ensure only one terminal window is open.
2. Run the command below:

<pre class="language-bash"><code class="lang-bash"><strong>rm -f ~/.zsh_history &#x26;&#x26; kill -9 $$
</strong></code></pre>

3. Close the terminal window.
4. Quit the terminal app by right-clicking the app icon and selecting “Quit”.
5. Open a terminal window.
6. Run the command below to ensure your `.zsh_history` is empty.

```bash
cat ~/.zsh_history
```

## <mark style="color:purple;">Clear Clipboard Content</mark>

```bash
pbcopy < /dev/null
```

## <mark style="color:purple;">List Local IP Address</mark>

The following command lists all local, non-loopback IPv4 addresses. Each IP address corresponds to a different network interface or network configuration on your machine.

```bash
ifconfig | grep "inet " | grep -Fv 127.0.0.1 | awk '{print $2}'
```

## <mark style="color:purple;">List Public IP Address</mark>

```bash
curl ifconfig.me
```

## <mark style="color:purple;">SSH</mark>

Access a remote machine via SSH.

```markup
ssh <REMOTE_USERNAME>@<REMOTE_IP_ADDRESS>
```

Exit SSH Session.

```bash
exit
```

In case your shell does not display `user@hostname`, the following are commands to check this information.

Check the current user's username:

```bash
whoami
```

Check the current machine's hostname:

```bash
hostname -s
```
