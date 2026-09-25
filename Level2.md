# Bandit Level 2

## Objective

The password is stored inside a file whose filename contains spaces and begins with hyphens (`-`).

The filename is:

```text
--spaces in this filename--
```

Because of the unusual filename, a normal `cat` command may not work as expected. The file path needs to be specified explicitly.

## Key Technique

A relative path beginning with `./` can be used to tell the shell that the filename should be treated as a path rather than as a command option.

| Situation                | Technique                                |
| ------------------------ | ---------------------------------------- |
| Filename contains spaces | Escape the spaces with `\` or use quotes |
| Filename begins with `-` | Prefix the filename with `./`            |
| Need to read the file    | Use `cat` with the complete path         |

## Guided Command

The filename can be accessed with:

```bash
cat ./--spaces\ in\ this\ filename--
```

The `./` is important here because the filename begins with `--`.

### Alternative Syntax

Quoting the complete path also makes the command easier to read:

```bash
cat './--spaces in this filename--'
```

Both approaches refer to the same file.

## What to Look For

After running the command, the contents of the file will be displayed in the terminal. The required password is contained in that output.

> **Hint:** The important lesson is how `./` changes the way the shell interprets a filename beginning with `-`.

## Takeaway

Special filenames can require special handling:

```text
./filename
```

Using `./` explicitly identifies the target as a file in the current directory, which is particularly useful when the filename begins with one or more hyphens.
