# git

## Introduction

{% hint style="info" %}
The git version: 2.24.0
{% endhint %}

## Working with stash

Show the content of a second last stash:

```text
g stash show -p stash@{1}
```

Apply the content of a second last stash:

```text
g stash apply stash@{1}
```

I prefer `apply` over `pop` as it does not remove content from stash stack

## Inspect changes from a given file

```text
git log -p myfile.yaml
```

## Revert changes

### Revert changes from few commits

```bash
git revert --no-commit 3251b46b8f9b1cfa23c991a5b70ca10ea1dd3880..HEAD
```

It reverts changes to the state of 3251b46b8f9b1cfa23c991a5b70ca10ea1dd3880

### Revert merge commit

```bash
git revert -m 1 62c97c7d1332ef454c044bdf62e029d2b92b0e83
```

The `-m` option specifies the parent number of merge commit

