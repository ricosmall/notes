# Git Rebase Upstream

## Steps

### Step 1: add the remote repository (original rempository that you forked from) and name it with 'upstream'.

```sh
git remote add upstream https://github.com/<owner>/<repo-name>.git
```

### Step 2: Fetch all branches of upstream from remote.

```sh
git fetch upstream
```

### Step 3: Rebase your local branch with upstream's branch.

```sh
git checkout <branch-name>
git rebase upstream/<branch-name>
```

### Step 4: Push your updates to remote.

```sh
git push origin <branch-name> --force
```
