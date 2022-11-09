# Remove ignored file from index

```shell
git rm --cached <path>
```

# Rebuild Git index

Removes all tracked files that should be ignored

```shell
git rm -r --cached .
git add .
```

# Recreate local branch from remote

 `@{u}` represents the upstream branch that the current branch is tracking

```shell
git reset --hard @{u}
```
