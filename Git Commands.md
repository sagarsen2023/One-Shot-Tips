# Basic Git Setup:
> Set your username
`git config --global user.name "Your Name"`

> Set your email
`git config --global user.email "your.email@example.com"`

> Set default branch name to 'main'
`git config --global init.defaultBranch main`


# Repository Setup:
> Initialize a new Git repository
`git init`

> Clone an existing repository
`git clone https://github.com/username/repository.git`


# Basic git commands:
> Check the status of your repository
`git status`

> Add files to staging area
`git add filename`
`git add .  # Add all files`

> Commit changes
`git commit -m "Your commit message"`

> View commit history
`git log`

# Branching
> Create a new branch
`git branch branch-name`

> Switch to a branch
`git checkout branch-name`

> Create and switch to a new branch
`git checkout -b new-branch-name`

> List all branches
`git branch -a`

> Delete a branch
`git branch -d branch-name`

# Merging:
> Merge a branch into the current branch
`git merge branch-name`

> Abort a merge in case of conflicts
`git merge --abort`


# Remote Repositories
> Add a remote repository
`git remote add origin https://github.com/username/repository.git`

> View remote repositories
`git remote -v`

> Fetch changes from a remote repository
`git fetch origin`

> Pull changes from a remote repository
`git pull origin branch-name`

> Push changes to a remote repository
`git push origin branch-name`

# Github Specific Interactions
> Fork a repository on GitHub (done through the GitHub interface)

> Clone your forked repository
`git clone https://github.com/your-username/forked-repository.git`

> Add the original repository as a remote (usually called 'upstream')
`git remote add upstream https://github.com/original-owner/original-repository.git`

> Create a pull request (done through the GitHub interface after pushing your changes)


# Advanced Git Commands
> Stash changes
`git stash`
`git stash pop`

> Rebase
`git rebase branch-name`

> Cherry-pick a commit
`git cherry-pick commit-hash`

> Reset to a specific commit
`git reset --hard commit-hash`

> Remove pushed commit after resetting
`git push origin HEAD --force`

> Create a tag
`git tag -a v1.0 -m "Version 1.0"`

> Push tags to remote
`git push --tags`

# GitHub CLI Commands:
> Authenticate with GitHub
`gh auth login`

> Create a new repository
`gh repo create`

> Clone a repository
`gh repo clone username/repository`

> Create a pull request
`gh pr create`

> List pull requests
`gh pr list`

> Check out a pull request locally
`gh pr checkout PR-number`

> View the diff of a pull request
`gh pr diff PR-number`
