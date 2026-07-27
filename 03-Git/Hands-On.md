**Install git & signup to GitHub**
Set up git identity
git config --global user.name "iozak"
git config --global user.email "iozak@github.com"

**SSH Key**
ssh-keygen -t ed25519 -C "iozak@github.com" -f ~/.ssh/handsongit
cat ~/.ssh/handsongit.pub - copy key
GitHub > Settings > Add SSH key

**New Repository**
mkdir handsongit then cd into it
git init
echo "# Git Hands-On" > README.md
git add .
git commit -m "init commit"
create a new repo on github
git remote add origin git@github.com:iozak/handsongit.git

**First Repository Push**
ssh -T git@github.com - verify connection
Had to run below to get above to succeed
```
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/handsongit
```
git push -u origin main
create .gitignore add lines '.env' '.DS_Store' - required for files/directories to ignore
git status - to track changes
git add .gitignore - then git status again to see the new file turns green
git commit -m "added gitignore"
git push

**Branching & Merging**
Added and commit txt file on main branch then used 'git checkout -b feature' to create a new branch named feature. Also added txt file with same name but different values in the txt file. Then worked through errors on resolving merge conflict. 'git branch -d feature' to delete the new branch at the end.

**Git WorkFlow**
Always git pull to make sure up to date.
'git checkout -b feature/add-about-page' - work on new branches when developing. When pushing from branch, it will prompt to set the upstream, can copy out command.
This will give a URL to view on GitHub. Here you can review, assign reviewers, then accept into the main branch.

**Undoing in Git**
'git restore undo.txt' - to undo changes made in the file back to the last commit.
'git restore --staged undo.txt' - this will undo the staging of a file.
'git reset --soft HEAD~1' - move the current branch back by one commit, but keep all changes from that commit staged for the next commit.
'git reset --mixed HEAD~1' - this will not keep changes staged. But will keep the changes in files.
'git reset --hard HEAD~1' - this will not keep changes in files.
'git revert HEAD' -  creates a new commit that undoes the changes by the most recent commit

**Git stash**
Temporarily saves your uncommitted changes and restores your working directory to a clean state, when needing to deal with other priority issues.
'git stash list' - view saved stashes
'git stash push -m "WIP: stash.md changes"' - after git add the new files run this to stash
'git stash apply' - restore stash but keep it saved
'git stash pop' - restore stash and remove it from list
'git stash clear' - removes all stashes

**Git rebase & squash**
Moves or reapplies commits onto a different base commit, creating a cleaner, linear history.
'git rebase -i HEAD~3' - this opens the last 3 commits. Then we can reorder, edit, squash, or drop before we push.

**Git cherry-pick**
'git cherry-pick commit-hash' - Allows to pick a particular commit from one branch to another

**The “.gitignore”**
Should also have .gitignore file populate with relevant files and directories to be excluded from any commit. This prevents accidently leaking anything sensitive.

**Git amend**
'git commit --amend' - modify most recent commit without creating a new one. Fix message, add/remove files.

**Pre-commit**
Requires separate framework. 'pip install pre-commit' along with a .yaml file present in git repo. This allows to run automated quality checks before changes are committed.












