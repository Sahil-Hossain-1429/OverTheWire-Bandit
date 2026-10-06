# Bandit Level 17

## Objective

There are two files in the home directory:

| File            | Purpose                                                    |
| --------------- | ---------------------------------------------------------- |
| `passwords.old` | Previous version of the password file                      |
| `passwords.new` | Updated version containing the password for the next level |

The password for the next level is the **only line that has changed** between the two files.

## Step 1: Compare the Files

Use the `diff` command:

```bash
diff passwords.old passwords.new
```

The output may look similar to this:

```text
42c42
< ################################
---
> ################################
```

## Understanding the Output

The output can be understood as follows:

| Output  | Meaning                                                           |
| ------- | ----------------------------------------------------------------- |
| `42c42` | Line 42 in the first file differs from line 42 in the second file |
| `<`     | Shows the line from `passwords.old`                               |
| `>`     | Shows the corresponding line from `passwords.new`                 |
| `---`   | Separates the two versions of the changed line                    |

In this example, the important part is the line marked with `>` because it represents the changed line in `passwords.new`.

The actual output will contain the password for the next level. Since the password is intentionally not displayed in this guide, the value should be read directly from the terminal.

## Step 2: Identify the Changed Line

Run:

```bash
diff passwords.old passwords.new
```

Then look for the line beginning with:

```text
>
```

That line represents the updated content in `passwords.new`.

For example:

```text
42c42
< old_content
---
> changed_content
```

The value represented by `changed_content` is the line that needs to be identified.

## Key Concept

The `diff` command compares two files and reports differences between them.

For this level:

```text
passwords.old
      ↓
   compare
      ↑
passwords.new
```

Only one line has changed, so the changed line shown from `passwords.new` contains the required password.

## Command Summary

| Purpose                        | Command                                |
| ------------------------------ | -------------------------------------- |
| Compare both files             | `diff passwords.old passwords.new`     |
| Identify the updated line      | Look for the line beginning with `>`   |
| Obtain the next level password | Read the changed value shown after `>` |

This approach reveals the location of the changed content without including the actual password in the guide.
