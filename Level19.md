# Bandit Level 19

## Objective

The goal of this level is to use the `setuid` binary located in the home directory to execute a command with the permissions of the next Bandit user.

The password for the next level is stored in the usual password directory:

```text
/etc/bandit_pass/
```

The password file itself should not be displayed directly in the guide. Instead, the required commands and the underlying concept are provided as a guided solution.

## Step 1: Identify the Setuid Binary

A `setuid` binary is available in the home directory.

Running the binary without arguments displays information about its usage.

```bash
./bandit20-do
```

The output provides an example similar to:

```text
Run a command as another user.
Example: ./bandit20-do whoami
```

This indicates that the binary can execute another command with the privileges associated with the next user.

## Step 2: Confirm the Effective User

The example from the binary can be used to verify the account under which the command executes.

```bash
./bandit20-do whoami
```

The result should indicate the next Bandit user.

This confirms that the `setuid` binary is executing commands with elevated permissions.

## Step 3: Read the Next Password File

The password file for the next level is located at:

```text
/etc/bandit_pass/bandit20
```

The current account does not have sufficient permissions to read this file directly.

The `bandit20-do` binary can execute `cat` with the required permissions:

```bash
./bandit20-do cat /etc/bandit_pass/bandit20
```

The command outputs the password required for the next level.

## Key Concept

The important concept in this level is **setuid**.

A setuid executable can run with the permissions of its file owner rather than the permissions of the account executing it.

In this level:

```text
bandit19
    |
    v
bandit20-do
    |
    v
command executed with bandit20 permissions
    |
    v
/etc/bandit_pass/bandit20
```

This makes it possible to read a file that is normally inaccessible to the `bandit19` account.

## Useful Commands

| Command                                       | Purpose                      |
| --------------------------------------------- | ---------------------------- |
| `./bandit20-do`                               | Display usage information    |
| `./bandit20-do whoami`                        | Check the effective user     |
| `./bandit20-do cat /etc/bandit_pass/bandit20` | Read the next level password |

## Takeaway

The key lesson from Bandit Level 19 is understanding how a `setuid` executable can perform operations using the permissions of another account.

Rather than accessing the protected password file directly, the provided executable is used as the permitted interface for running the required command.
