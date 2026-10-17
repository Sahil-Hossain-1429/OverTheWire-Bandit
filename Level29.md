# Bandit Level 29: Exploring Git Branches

## Objective

The Git repository for Bandit Level 29 contains a `README.md` file with a placeholder instead of the next level's password in the production branch.

The password may be available in another branch. The objective is to explore the repository's branches and locate the relevant information.

## Step 1: Clone the Git Repository

Run the following command from a local machine, not from the OverTheWire server.

```bash
git clone ssh://bandit29-git@bandit.labs.overthewire.org:2220/home/bandit29-git/repo
```

When prompted, enter the password for Bandit Level 29.

| Command component | Description |
|---|---|
| `git clone` | Creates a local copy of the remote repository. |
| `ssh://` | Specifies the SSH protocol for repository access. |
| `bandit29-git@bandit.labs.overthewire.org` | Specifies the repository account and remote host. |
| `:2220` | Specifies the SSH port used by the challenge. |
| `/home/bandit29-git/repo` | Specifies the remote repository path. |

## Step 2: Enter the Repository

Navigate into the newly cloned repository.

```bash
cd repo
```

Display the contents of the README file.

```bash
cat README.md
```

The file identifies the next account but indicates that no password is available in the production branch.

## Step 3: List All Available Branches

Inspect the local and remote branches.

```bash
git branch -a
```

Example output:

```text
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/dev
  remotes/origin/master
  remotes/origin/sploits-dev
```

| Branch | Description |
|---|---|
| `master` | The current local branch. |
| `origin/master` | The remote tracking branch for the default branch. |
| `origin/dev` | A remote development branch that may contain additional information. |
| `origin/sploits-dev` | Another remote development branch worth inspecting. |
| `origin/HEAD` | A symbolic reference indicating the remote repository's default branch. |

**Important:** A Git repository can contain different file contents across branches. Examining the default branch alone may not reveal all available information.

## Step 4: Inspect the Development Branch

Switch to the development branch using the remote tracking branch name.

```bash
git checkout -b dev origin/dev
```

This command creates a local branch named `dev` based on `origin/dev` and switches to it.

Display the README file again.

```bash
cat README.md
```

Compare the contents with the README file from the default branch and check whether the relevant information has changed.

## Step 5: Inspect Other Branches if Necessary

If the required information is not available in the development branch, inspect the remaining remote branches.

For example, switch to the `sploits-dev` branch:

```bash
git checkout -b sploits-dev origin/sploits-dev
```

Then examine the README file:

```bash
cat README.md
```

The same process can be repeated for other branches when necessary.

## Command Summary

| Purpose | Command |
|---|---|
| Clone the repository | `git clone ssh://bandit29-git@bandit.labs.overthewire.org:2220/home/bandit29-git/repo` |
| Enter the repository | `cd repo` |
| Read the README file | `cat README.md` |
| List all branches | `git branch -a` |
| Create and switch to the development branch | `git checkout -b dev origin/dev` |
| Create and switch to another remote branch | `git checkout -b sploits-dev origin/sploits-dev` |
| Check the current branch | `git branch --show-current` |

## Key Takeaway

Git branches can contain different versions of the same file. When a repository's default branch does not contain the required information, inspecting other branches is a useful troubleshooting technique.

The same approach applies to repository analysis, version control investigations, and debugging unexpected differences between development and production code.