# Bandit Level 27: Cloning a Git Repository

## Level Objective

A Git repository is available on the OverTheWire server. The objective is to clone the repository from a local machine, inspect its contents, and locate the password for the next level.

The repository can be accessed using the following SSH address:

```bash
ssh://bandit27-git@bandit.labs.overthewire.org/home/bandit27-git/repo
```

**Important details**

| Item | Description |
| :--- | :--- |
| Git username | `bandit27-git` |
| Server | `bandit.labs.overthewire.org` |
| Port | `2220` |
| Repository path | `/home/bandit27-git/repo` |
| Authentication | Password for Bandit Level 27 |

## Step 1: Exit the Current SSH Session

If an SSH session is active on the OverTheWire machine, exit the session first.

```bash
exit
```

The repository must be cloned from a local machine, not from the OverTheWire server.

## Step 2: Clone the Repository

Run the following command in the local terminal:

```bash
git clone ssh://bandit27-git@bandit.labs.overthewire.org:2220/home/bandit27-git/repo
```

When prompted, enter the password associated with Bandit Level 27.

The command downloads the repository into a new directory named `repo`.

## Step 3: Enter the Repository Directory

Move into the newly created directory:

```bash
cd repo
```

## Step 4: Inspect the Repository Contents

List the files and directories in the repository:

```bash
ls
```

Look for a file named `README`.

Display its contents using:

```bash
cat README
```

The README file contains the information needed to proceed to the next level.

## Understanding the Git Clone Command

| Command component | Explanation |
| :--- | :--- |
| `git clone` | Creates a local copy of a remote Git repository. |
| `ssh://` | Specifies SSH as the connection protocol. |
| `bandit27-git@` | Identifies the remote SSH username. |
| `bandit.labs.overthewire.org` | Specifies the remote server hostname. |
| `:2220` | Specifies the SSH port. |
| `/home/bandit27-git/repo` | Specifies the repository location on the server. |

## Key Takeaways

1. Git repositories can be cloned over SSH.
2. SSH URLs can specify a custom port and a remote filesystem path.
3. The `git clone` command creates a local copy of the repository.
4. The `cd` command changes the current directory.
5. The `ls` and `cat` commands help inspect repository contents.

The objective is to understand how SSH authentication and Git cloning work together to access a remote repository and retrieve the information required for the next Bandit level.