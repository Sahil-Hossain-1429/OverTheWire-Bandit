# Bandit Level 6

## Objective

The password for the next level is stored somewhere on the server.

The exact directory is not provided, but the password file has the following properties:

| Property  | Required value |
| --------- | -------------- |
| Owner     | `bandit7`      |
| Group     | `bandit6`      |
| File size | `33 bytes`     |

The `find` command can search the filesystem and filter files according to these properties.

## Understanding the Search Location

In Linux, `/` represents the root directory of the filesystem.

Searching from `/` allows the `find` command to examine the entire server filesystem.

The required `find` options are:

| Option   | Purpose              | Value     |
| -------- | -------------------- | --------- |
| `-user`  | Search by file owner | `bandit7` |
| `-group` | Search by file group | `bandit6` |
| `-size`  | Search by file size  | `33c`     |

The `c` after `33` specifies that the size is measured in bytes.

## Building the Command

The basic search command is:

```bash
find / -user bandit7 -group bandit6 -size 33c
```

This searches the filesystem starting from `/` and looks for a file that matches all three conditions.

## Handling Permission Errors

Because the search begins at `/`, some directories may not be accessible to the current account.

As a result, the terminal can display several permission errors while the search is running.

Those error messages can be redirected to `/dev/null`:

```bash
find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
```

### Understanding `2>/dev/null`

Linux provides three standard streams:

| Number | Stream   | Meaning         |
| ------ | -------- | --------------- |
| `0`    | `stdin`  | Standard input  |
| `1`    | `stdout` | Standard output |
| `2`    | `stderr` | Standard error  |

The expression:

```bash
2>/dev/null
```

means that standard error is redirected to `/dev/null`.

`/dev/null` acts as a destination where unwanted output can be discarded.

For example:

```bash
command 2>/dev/null
```

runs a command while suppressing error messages.

Normal command output remains visible because only stream `2`, standard error, is being redirected.

## Final Command

The complete command for the level is:

```bash
find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
```

The resulting output identifies the file matching the required ownership, group, and size conditions.

The contents of that file can then be examined to continue to the next Bandit level.

## Key Concepts

| Concept     | Explanation                        |
| ----------- | ---------------------------------- |
| `/`         | Root of the Linux filesystem       |
| `find`      | Searches for files and directories |
| `-user`     | Filters by file owner              |
| `-group`    | Filters by file group              |
| `-size`     | Filters by file size               |
| `2>`        | Redirects standard error           |
| `/dev/null` | Discards redirected output         |

The important technique in this level is combining multiple `find` filters and redirecting permission errors so that the relevant search result remains easy to identify.

source: https://askubuntu.com/questions/1322487/how-to-ignore-error-in-shell-script
