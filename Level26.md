# Bandit Level 26 to Level 27

## Problem Description

The objective is to retrieve the password for **Bandit Level 27** after successfully obtaining a shell from the previous level.

## Step 1: Identify the Available Files

After connecting to Bandit Level 26, inspect the available files using:

```bash
ls
```

The directory contains the following files:

| File Name | Description |
|---|---|
| `bandit27-do` | A program that can execute commands with the permissions of the Bandit Level 27 account. |
| `text.txt` | A text file that may contain information related to the level. |

## Step 2: Execute the Provided Program

The `bandit27-do` program can be used to execute commands with elevated privileges. This makes it possible to read the password file for the next level.

First, inspect the program's available behavior:

```bash
./bandit27-do
```

Review any usage information displayed before proceeding.

## Step 3: Read the Password File

Use the provided program with the `cat` command to access the password file for Bandit Level 27:

```bash
./bandit27-do cat /etc/bandit_pass/bandit27
```

### Understanding the Command

| Command Component | Purpose |
|---|---|
| `./bandit27-do` | Executes the provided program from the current directory. |
| `cat` | Displays the contents of a file. |
| `/etc/bandit_pass/bandit27` | Specifies the password file for Bandit Level 27. |

## Step 4: Complete the Level

If the command executes successfully, the terminal displays the password for Bandit Level 27.

Copy the displayed value and use it to authenticate to the next level.

## Key Takeaways

1. The `ls` command helps identify available files in the current directory.
2. A program with elevated privileges can provide access to files that would otherwise be restricted.
3. The `cat` command displays the contents of a readable file.
4. Understanding how to execute a provided program is an important skill in Linux privilege management.

**Level 26 complete!** The password for Bandit Level 27 can now be used to continue the OverTheWire Bandit challenge.