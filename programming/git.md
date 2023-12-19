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

## <mark style="color:purple;">Commits</mark>

### Revert to Previous Commit

```markup
git reset --hard <COMMIT_HASH>
```

### Revert all Unstaged Changes

The following command will revert all unstaged changes on the current branch. For example, any changes seen when running `git status` before staging will be reverted when running the command below.

```markup
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

```markup
git config user.name <USER_NAME>
git config user.email <EMAIL>
```
