# A **simple** git branching model

This is a very simple git workflow. It (and variants) is in use by many people.
It's all referenced from https://gist.github.com/17twenty/6733076


### The gist

1. `master` must always be deployable.
2. **all changes** made through feature branches (pull-request + merge)
3. rebase to avoid/resolve conflicts; merge in to `master`
4. merge feature branches back in to master with --no-ff flag to keep the history

### The workflow

```
# everything is happy and up-to-date in master
git checkout master
git pull origin master

# let's branch to make changes
git checkout -b my-new-feature

# go ahead, make changes now.
$EDITOR file

# commit your (incremental, atomic) changes
git add -p
git commit -m "my changes"

# keep abreast of other changes, to your feature branch or master.
# rebasing keeps our code working, merging easy, and history clean.
git fetch origin
git rebase origin/my-new-feature
git rebase origin/master

# optional: push your branch for discussion (pull-request)
#           you might do this many times as you develop.
git push origin my-new-feature

# optional: feel free to rebase within your feature branch at will.
#           ok to rebase after pushing if your team can handle it!
git rebase -i origin/master

# merge when done developing.
# --no-ff preserves feature history and easy full-feature reverts
# merge commits should not include changes; rebasing reconciles issues
# github takes care of this in a Pull-Request merge
git checkout master
git pull origin master
git merge --no-ff my-new-feature


# optional: tag important things, such as releases
git tag 1.0.0-RC1
```

### Useful config

```
# autosetup rebase so that pulls rebase by default
git config --global branch.autosetuprebase always

# if you already have branches (made before `autosetuprebase always`)
git config branch.<branchname>.rebase true
```

### DOs and DON'Ts

No DO or DON'T is sacred. You'll obviously run into exceptions, and develop 
your own way of doing things. However, these are guidelines I've found
useful.

#### DOs

- **DO** keep `master` in working order.
- **DO** rebase your feature branches.
- **DO** pull in (rebase on top of) changes
- **DO** tag releases
- **DO** push feature branches for discussion
- **DO** learn to rebase


#### DON'Ts

- **DON'T** merge in broken code.
- **DON'T** commit onto `master` directly.
- **DON'T** hotfix onto master! use a feature branch.
- **DON'T** rebase `master`.
- **DON'T** merge with conflicts. handle conflicts upon rebasing.


### Branch Naming

Branches created should be named using the following format:

```
{story type}/{pivotal tracker id}-{2-3 word summary}
```

`story-type` - Indicates the context of the branch and should be one of:

- feature
- bug
- hotfix
- release

`story-summary` - Short 2-3 words summary about what the branch contains

**Example**

```
feature/111504508-resources-rest-endpoints
```

### PR Naming

The PR title should be named using the following format:

```
#[STORY_ID] Story description
```

**Example**

```
#111504508 Build out REST Endpoints for Resources (CRUD)
```

### PR Description Template (Markdown)

The description of the PR should contain the following headings and corresponding content in Markdown format.

```md
#### What does this PR do?
#### Description of Task to be completed?
#### How should this be manually tested?
#### Any background context you want to provide?
#### What are the relevant pivotal tracker stories?
#### Screenshots (if appropriate)
#### Questions:
```

### Commits

Atomic commits should be made with the format:

```
[story-type #[pivotal-tracker-id]] Description of task that has been implemented.
```

`story-type` - Indicates the context of the branch. Should be one of:

- Feature
- Bug
- Hotfix
- Release

**Example**

```
[Feature #111504652] Add status code assertion in tests
```

### Git Aliases
You can add these to your ~/.bash_profile, ~/.bashrc or ~/.zshrc:

```
alias gs='git status '
alias ga='git add '
alias gb='git branch '
alias gc='git commit'
alias gca='git commit -a'
alias gd='git diff'
alias go='git checkout '
alias gk='gitk --all&'
alias gx='gitx --all'
alias glo="git log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```
