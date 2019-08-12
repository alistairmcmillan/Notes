# Git Notes

## How to update a forked repository

From Terminal...

- `git remote -v` to get names of repositories
- `git fetch THEIR_REPO` to update copy of original repo
- `git merge THEIR_REPO/BRANCHNAME` to merge changes
- `git push MY_REPO/BRANCHNAME` to push merged repo to GitHub

## How to revert to a previous commit

- `git reset --hard HASH`

## How to force push to wipe out remote changes

- `git push -f <remote> <branch>`
