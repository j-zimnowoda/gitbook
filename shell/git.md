# git

## Revert changes

### Revert changes from few commits

```text
git revert --no-commit 3251b46b8f9b1cfa23c991a5b70ca10ea1dd3880..HEAD
```

It reverts changes to the state of 3251b46b8f9b1cfa23c991a5b70ca10ea1dd3880

### Revert merge commit

```text
git revert -m 1 62c97c7d1332ef454c044bdf62e029d2b92b0e83
```

The `-m` option specifies the parent number of merge commit

