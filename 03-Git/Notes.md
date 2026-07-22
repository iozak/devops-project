**What is Version Control**
You can undo your mistakes, see who changed what, and collaborate. For DevOps, this isn't just about application code.. Think Terraform files, modules, Kubernetes manifests, Dockerfiles, Ansible roles, CI/CD configs.

**Centralised vs Distributed**
With Git being distributed it does not rely on a single server, everyone has a copy of the repo, including entire history. You can work offline, branch as much you want, and merge changes later. Allow for real collaboration. 

**Understanding Git Architecture**
Git is not just a file tracker. What Git tracks are snapshots. Under the hood, Git uses SHA1 hashes, these uniquely represent your content. It's a smart, compressed, hash-based snapshot engine.

**Git Terminology**
![[GitTerminology.png]]

**The .git Directory**
![[GitDirectory.png]]

**Git Common Commands**
![[GitCommonCommands.png]]

**The Areas of Git**
(Working Directory) > git add >  (Staging Area) > git commit > Repository.

**Viewing History**
'git log' see commit history
'git log --oneline --graph' visual summary
'git show "commit"' view a specific commit
'git diff' compare unstaged vs last commit
'git diff --staged' compare staged vs last commit
'git blame "file"' show who last changes each line
'git reflog' view local HEAD history (even deleted brances)

**Git Vs GitHub - What's the Difference**
Git is the foundation. GitHub is a platform. Git is the actual version control tool, the CLI that makes those changes. GitHub on the other hand, hosts repos, allows for collaboration. 

**Branching 101**
'git branch' list/create branches
'git checkout -b "branch"' create & switch (older syntax)
'git switch -c "branch"' modern version
'git switch "branch"' switch branches safely
'git branch -d "branch"' delete branch

**Merging**
Git can handle merging in 2 ways. Fast forward merge, it basically moves the pointer forward. Think of it as Main branch a>b>c and branch1 as d>e merging these would get you a clean a>b>c>d>e. Now if changes happened on the main branch after branch1 was created this becomes a different situation.

**Visualise Branches & Logs**
'git log --oneline' compact commit view
'git log --graph' visual tree structure
'git log --oneline --graph --all' best use. visual, full view
This is great for debugging merges and tracking branches.

**Merge Vs Rebase**
Merge. Preserves history. Creates a new commit. Good for team workflows. Use this when working collaboratively. 
Rebase. Rewrites history (linear). No merge commits. Ideal for cleaning up before PR. Use this when cleaning up your local history.

**Git Stash & Pop**
'git stash' temp save uncommitted changes
'git stash list' view all stashes
'git stash apply' reapply latest latest (keeps stash)
'git stash pop' reapply and delete the stash
Use when switching branches mid task.
Great for I'm not ready to commit, but I need to move.

**Revert, Reset and Cherry-Pick**
![[GitRevertResetCherrypick.png]]

**Forks & Pull Requests**
Fork = your own copy of someone else's repo (on GitHub)
Clone the forked repo to your local machine
Make changes > pushes to fork
Open a pull request (PR) to propose your changes
Used in open source and cross team workflows
Original repo owner can review, comment, and merge

**Collaborating Practices**
Use branches to isolate work. Push to remote and open pull requests.
Assign reviewers, use GitHub's UI for comments. Resolve conflict before merge.
Use Issues, Projects, Discussions to track work.
Keep commits focused and clean.

**Typical Git Workflow**
Developer pulls latest main or clones repo
⌄
Creates feature branch
⌄
Works locally > commits > pushes branch
⌄
Opens PR/MR > review or merge
⌄
Teams sync regularly via git pull --rebase or merge

**Trunk-Based Development**
All devs commit to main or short lived branches.
Heavy CI/testing gates.
Used in fast moving orgs (example, Google, Facebook).

**Commit Hygiene & Best Practices**
Write good commit messages.
Use squashing before merging PRs.
One logical change per commit.

**Pre-Commit & Automation**
Run linters/tests before commiting (pre-commit, Husky tflink, tfsec. etc).
Prevent broken code from entering the repo.
Hook into CI pipelines for formatting, testing, & scanning.

**Common Mistakes in the Real World**
Forgetting to pull before pushing.
Force pushing to shared branches (unless necessary)
Committing secrets or sensitive files.
Merging without reviewing.
Not using .gitignore properly.