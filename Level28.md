# Bandit Level 28 → Level 29

## 1. Clone the Git Repository

The Git repository for this level is available through SSH on port `2220`. The repository must be cloned from a local machine, not from the OverTheWire Bandit server.

Run the following command:

```bash
git clone ssh://bandit28-git@bandit.labs.overthewire.org:2220/home/bandit28-git/repo
```

When prompted, enter the password for `bandit28`, which is also the password for `bandit28-git`.

## 2. Enter the Repository

After the repository has been cloned successfully, enter the repository directory:

```bash
cd repo
```

The repository contains a file named `README.md`.

## 3. Inspect the README File

Display the contents of the file:

```bash
cat README.md
```

The output contains notes for Bandit Level 29, including the username and password fields.

However, the password has been censored in the current version of the file.

## 4. Inspect the Git Commit History

Git stores a history of changes made to tracked files. Earlier commits may contain information that is no longer visible in the latest version.

Display the commit history:

```bash
git log
```

Review the commit messages and look for a commit related to the missing data. In this level, a relevant commit message is:

```text
add missing data
```

Copy the full commit hash associated with that entry.

## 5. Check Out the Earlier Commit

Switch to the identified commit using its hash:

```bash
git checkout <commit-hash>
```

Replace `<commit-hash>` with the actual commit hash copied from the Git history.

This command checks out the repository at the selected historical commit. Git may display a message indicating that the repository is in a detached HEAD state, which is normal when checking out a specific commit directly.

## 6. Inspect the README Again

Display the README file once more:

```bash
cat README.md
```

The historical version may contain the information that was removed or censored in the latest commit.

The password for Bandit Level 29 can then be identified from the earlier version of the file.

## Key Takeaway

Git preserves a repository's commit history, allowing earlier versions of files to be inspected even when information has been removed from the latest version.

For this level, the main concept is to use `git log` to locate a relevant historical commit and `git checkout` to inspect the file as it existed at that point in the repository's history.