---
description: Useful Git Commands
---

# Git

## <mark style="color:purple;">Repositories</mark>

### Create a New Repo

```markup
git init
git add -A
git commit -m "first commit"
git branch -M main
git remote add origin <REPO_LINK>
git push -u origin main
```

### Push an Existing Repo

```markup
git remote add origin <REPO_LINK>
git branch -M main
git push -u origin main
```

## <mark style="color:purple;">Branches</mark>

### Create a Branch

The branch will be created locally, meaning that  it will not be visible on GitHub.com until it's pushed.

{% code overflow="wrap" %}
```markup
git checkout -b <BRANCH_NAME>
```
{% endcode %}

### Switch a Branch

Cannot switch to another branch if uncommitted changes exist on the current branch.

```markup
git checkout <BRANCH_NAME>
```

### Push to Branch

```markup
git push origin <BRANCH_NAME>
```

### Delete a Branch

```markup
git branch -D <BRANCH_NAME>
```

### Rename a Local Branch

```markup
git branch -m <OLD_BRANCH_NAME>  <NEW_BRANCH_NAME>
```

### Unstage All Changes on Local Branch

```bash
git reset
```

### Unstage a File on Local Branch

```markup
git reset <FILE_PATH>
```

### Divergent Branches

Sometimes, when attempting to pull changes on a branch, an error occurs with the following information:

```bash
hint: You have divergent branches and need to specify how to reconcile them.
hint: You can do so by running one of the following commands sometime before
hint: your next pull:
hint: 
hint:   git config pull.rebase false  # merge
hint:   git config pull.rebase true   # rebase
hint:   git config pull.ff only       # fast-forward only
hint: 
hint: You can replace "git config" with "git config --global" to set a default
hint: preference for all repositories. You can also pass --rebase, --no-rebase,
hint: or --ff-only on the command line to override the configured default per
hint: invocation.
```

The following command is the preferred way to ensure a clean, linear history. It will fetch the changes from the remote branch, and instead of merging the fetched changes into the local branch, it applies the local changes on top of the changes retrieved.

```markup
git pull --rebase origin <BRANCH_NAME>
```

## <mark style="color:purple;">Commits</mark>

### Revert to Previous Commit

```markup
git reset --hard <COMMIT_HASH>
```

### Revert all Unstaged Changes

The following command will revert all unstaged changes on the current branch. For example, any changes seen when running `git status` before staging will be reverted when running the command below.

```bash
git checkout -- .
```

### Amend Last Commit

The following is useful in the scenerio where you need to make more changes right after pushing to GitHub, but don't want to create another commit.\
This is a way to add new changes to the last commit without editing the commit message.

```markup
git add -A
git commit --amend --no-edit
git push -f origin <BRANCH_NAME>
```

## <mark style="color:purple;">Accounts</mark>

### Display Username and Email in Current Repo

```markup
git config user.name
git config user.email
```

### Change Username and Email in Current Repo

This overwrites the global config username and email.

```markup
git config user.name <USER_NAME>
git config user.email <EMAIL>
```

### Change Username and Email Globally

This will be default config used if username and email are not set in a repo.

```markup
git config --global user.name <USER_NAME>
git config --global user.email <EMAIL>
```
