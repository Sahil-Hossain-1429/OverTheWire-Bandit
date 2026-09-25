# Bandit Level 3

## Objective

The password for the next level is stored inside a hidden file located in the `inhere` directory.

The solution requires navigating into the directory, displaying hidden files, and reading the appropriate file.

## Step 1: Enter the `inhere` Directory

The `cd` command is used to change the current working directory.

```bash
cd inhere/
```

The command above changes the current directory to `inhere`.

| Command      | Purpose                                   |
| ------------ | ----------------------------------------- |
| `cd inhere/` | Changes the current directory to `inhere` |

## Step 2: Display Hidden Files

The `ls` command lists files and directories in the current location.

A normal `ls` command does not display hidden files. The `-a` option can be added to display all files, including hidden files.

```bash
ls -a
```

Here, `-a` stands for `all`.

| Command | Purpose                                                 |
| ------- | ------------------------------------------------------- |
| `ls`    | Lists visible files and directories                     |
| `ls -a` | Lists all files and directories, including hidden files |

The hidden file can now be identified from the directory listing.

## Step 3: Read the Hidden File

Once the hidden file has been located, the `cat` command can be used to display its contents.

```bash
cat <hidden_file>
```

Replace `<hidden_file>` with the filename discovered in the previous step.

The displayed content contains the password required for the next Bandit level.

## Command Summary

| Step | Command             | Purpose                              |
| ---- | ------------------- | ------------------------------------ |
| 1    | `cd inhere/`        | Enter the `inhere` directory         |
| 2    | `ls -a`             | Display visible and hidden files     |
| 3    | `cat <hidden_file>` | Read the contents of the hidden file |

### Key Concepts

`cd` is used for changing directories.

`ls -a` is useful when hidden files need to be displayed.

`cat` is used to read the contents of a file.

The password itself is intentionally not included in the guide. The commands provide the necessary path to locate and retrieve it independently.
