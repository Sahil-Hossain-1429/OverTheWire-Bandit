# OverTheWire Bandit Level 30 Guide

## 1. Understanding the Challenge

The objective is to explore a Git repository and identify information that can help progress to the next level.

| Detail | Description |
|---|---|
| Repository user | `bandit30-git` |
| Repository host | `bandit.labs.overthewire.org` |
| SSH port | `2220` |
| Repository path | `/home/bandit30-git/repo` |
| Authentication | Password for Bandit Level 30 |
| Required tool | Git installed on the local machine |

## 2. Clone the Git Repository

The repository must be cloned from the local machine rather than from the OverTheWire server.

```bash
git clone ssh://bandit30-git@bandit.labs.overthewire.org:2220/home/bandit30-git/repo
```

Enter the password associated with Bandit Level 30 when prompted.

### Understanding the Command

| Component | Explanation |
|---|---|
| `git clone` | Creates a local copy of a Git repository |
| `ssh://` | Specifies SSH as the connection protocol |
| `bandit30-git@` | Identifies the remote Git account |
| `bandit.labs.overthewire.org` | Specifies the remote server |
| `:2220` | Specifies the SSH port |
| `/home/bandit30-git/repo` | Specifies the repository location |

## 3. Enter the Repository

After cloning completes, move into the repository directory.

```bash
cd repo
```

List the files to inspect the repository contents.

```bash
ls -la
```

Read the README file:

```bash
cat README.md
```

The output is:

```text
just an epmty file... muahaha
```

The README does not provide a useful clue. Further investigation of Git's metadata is necessary.

## 4. Inspect the Git Commit History

Git records changes through commits. The commit history can reveal information that is not immediately apparent from the current files.

Run:

```bash
git log
```

The visible history contains an initial commit associated with `README.md`.

| Command | Purpose |
|---|---|
| `git log` | Displays the commit history |
| `git log --oneline` | Displays commits in a compact format |
| `git show HEAD` | Displays details about the latest commit |

The available commit history does not appear to contain the required clue.

## 5. Inspect the Available Branches

Git branches provide separate lines of development. A repository may contain useful information outside its current branch.

Run:

```bash
git branch -a
```

The output shows:

```text
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/master
```

| Output | Explanation |
|---|---|
| `master` | The current local branch |
| `remotes/origin/master` | The remote tracking branch |
| `remotes/origin/HEAD` | A symbolic reference to the remote's default branch |

No additional branches are visible in this output.

## 6. Explore Git Tags

Git tags are references that identify particular points in a repository's history. They are often used to mark releases, versions, or other significant commits.

A tag can also contain an important clue in a security challenge.

List the available tags:

```bash
git tag
```

A tag named `secret` appears in the output.

Rather than inspecting another branch or commit, the next step is to investigate this tag.

Display the tag information:

```bash
git show secret
```

### Understanding the Command

| Component | Explanation |
|---|---|
| `git tag` | Lists the available tags |
| `git show` | Displays information about a Git object |
| `secret` | Specifies the tag to inspect |

The output of this command is the next area to examine. Review the displayed information carefully to determine how it relates to the challenge.

## 7. Key Takeaways

| Git Feature | What It Helps Investigate |
|---|---|
| README files | Repository documentation and initial clues |
| Commit history | Previous changes and commit messages |
| Branches | Alternative lines of development |
| Tags | Named references to specific points in repository history |
| `git show` | Details about commits, tags, and other Git objects |

**Important lesson:** When a repository contains no useful information in its files, commit history, or branches, Git tags provide another useful place to investigate.