---
description: Useful ZSH Commands
---

# ZSH

**NOTE:** The following commands are largely the same as BASH commands, since they're both UNIX-based shells.

## Basic Commands

### Change the Current Directory

```
cd <DIR_NAME>
```

### List Directory Contents

```
ls
```

### Move or Rename Directories

```
mv <SOURCE> <DESTINATION>
```

### Remove Files Directories

```
rm <FILE_OR_DIR_NAME>
```

### Set an Environment Variable

```
export VARIABLE_NAME=<SOME_VALUE>
```

### Display an Environment Variable

```
echo $VARIABLE_NAME
```

### Display an Entire File

```
cat <FILE_NAME>
```

### Display the First N Lines of a File

Fist 10 Lines:

<pre><code><strong>head &#x3C;FILE_NAME>
</strong></code></pre>

First 5 Lines:

<pre><code><strong>head -n 5 &#x3C;FILE_NAME>
</strong></code></pre>

### Show Command History

```
history
```

## Clear Terminal History

1. Ensure only one terminal window is open.
2. Run the command below:

<pre><code><strong>rm -f ~/.zsh_history &#x26;&#x26; kill -9 $$
</strong></code></pre>

3. Close the terminal window.
4. Quit the terminal app by right-clicking the app icon and selecting “Quit”.
5. Open a terminal window.
6. Run the command below to ensure your `.zsh_history` is empty.

```
cat ~/.zsh_history
```

## Clear Clipboard Content

```
pbcopy < /dev/null
```

## List Local IP Address

The following command lists all local, non-loopback IPv4 addresses. Each IP address corresponds to a different network interface or network configuration on your machine.

```
ifconfig | grep "inet " | grep -Fv 127.0.0.1 | awk '{print $2}'
```

## List Public IP Address

```
curl ifconfig.me
```

## SSH

Access a remote machine via SSH.

```
ssh <REMOTE_USERNAME>@<REMOTE_IP_ADDRESS>
```

Exit SSH Session.

```
exit
```

In case your shell does not display `user@hostname`, the following are commands to check this information.

Check the current user's username:

```
whoami
```

Check the current machine's hostname:

```
hostname -s
```
