# Bandit Level 5

## Objective

The password is stored somewhere inside the `inhere` directory.

The target file has three important properties:

| Property   | Requirement    |
| ---------- | -------------- |
| File type  | Human readable |
| File size  | 1033 bytes     |
| Executable | Not executable |

The `inhere` directory contains many subdirectories and files, so the main task is to locate the single file matching all three properties.

## Step 1: Find the files

The `find` command can search through the entire `inhere` directory.

```bash
find inhere/ -type f
```

The `-type f` option limits the results to regular files. Directories are excluded from the results.

## Step 2: Check whether the files are human readable

The `file` command can identify the type and format of each file.

```bash
find inhere/ -type f -exec file {} +
```

This produces information about all the files found inside `inhere`.

Since the target file is human readable, `grep` can filter the output for ASCII text files.

```bash
find inhere/ -type f -exec file {} + | grep ASCII
```

This reduces the results to files identified as ASCII text.

## Step 3: Filter by file size

The target file is exactly 1033 bytes.

The `-size` option can restrict the search to files of a specific size.

```bash
find inhere/ -type f -size 1033c -exec file {} + | grep ASCII
```

Here, `1033c` means exactly 1033 bytes, where `c` represents bytes.

At this stage, the search should narrow down to the relevant file.

## Step 4: Include the executable requirement

The final property is that the file must not be executable.

The `!` operator can be used with `find` to negate a condition.

```bash
find inhere/ -type f -size 1033c ! -executable -exec file {} + | grep ASCII
```

The important parts of the command are:

| Part              | Purpose                                  |
| ----------------- | ---------------------------------------- |
| `find inhere/`    | Searches inside the `inhere` directory   |
| `-type f`         | Selects regular files                    |
| `-size 1033c`     | Selects files exactly 1033 bytes in size |
| `! -executable`   | Excludes executable files                |
| `-exec file {} +` | Checks the file type                     |
| `grep ASCII`      | Keeps human readable ASCII text results  |

This combines all the known properties into a single search command and identifies the file containing the required information without displaying the password directly.

The resulting file can then be inspected to continue the Bandit level.
