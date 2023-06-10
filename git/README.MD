## Configure Git
```
git config --global user.name "Jean Gabin"
git config --global user.email "jean@gabin.fr"
```

## Clone a repo
```
git clone ssh://git@github.com/${project}/${repo}.git
```

## Send new code
```
cd existing-project
git init
git add --all
git commit -m "Initial Commit"
git remote add origin ssh://git@github.com/${project}/${repo}.git
git push -u origin master
```

## Send existig local repo
```
cd existing-project
git remote set-url origin ssh://git@github.com/${project}/${repo}.git
git push -u origin --all
git push origin --tags
```

## Checkout remote branch
```
git checkout --track origin/dev/$user/$branch
git checkout -b $branch origin/dev/$user/$branch
```

## Delete remove branch
```
git push origin :${branch}
```

## Find a deleted file in the project commit history
If you do not know the exact path you may use
```
git log --all --full-history -- **/thefile.*
```
If you know the path the file was at, you can do this:
```
git log --all --full-history -- <path-to-file>
```
This should show a list of commits in all branches which touched that file. Then, you can find the version of the file you want, and display it with...
```
git show <SHA> -- <path-to-file>
```
Or restore it into your working copy with://
```
git checkout <SHA>^ -- <path-to-file>
```
Note the caret symbol (^), which gets the checkout prior to the one identified, because at the moment of <SHA> commit the file is deleted, we need to look at the previous commit to get the deleted file's contents

## Diff under 2 remotes
```
git diff remotes/heroku/master remotes/staging/master
```

## Only the files that are different
```
git diff --stat --color master origin/master
```

## Get a list of the deleted files and copy the full path of the deleted file
```
git log --diff-filter=D --summary | grep delete
```

## Find commit id of that commit and copy the commit id
```
git log --all -- FILEPATH
```

## Show diff of deleted file
```
git show COMMIT_ID -- FILE_PATH > deleted.diff
```

## Show log by pattern
```
git log -S '164.132.119.9'
```

## Clear reflog
```
git reflog expire --expire=90.days.ago --expire-unreachable=now --all
```

## Removes untracked files from the working tree
```
git-clean
```

## Discard unstaged changes in Git?
```
git stash save ed
git stash dr
```

## Check commits by author
```
git shortlog
git shortlog -s -n
```

## Show merged commit
```
git show --first-parent
```

## Show branches
```
git log --graph --decorate --pretty=oneline --abbrev-commit --all
```
better use tig

## Change default branch refs
```
git remote set-head origin prod
```

## Delete specific commit
```
git rebase -p --onto SHA^ SHA
```

## submodule update
```
git submodule update --remote --merge $submodulename
```

## misc
https://sethrobertson.github.io/GitFixUm/fixup.html

