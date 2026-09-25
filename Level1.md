# Bandit Level 1

## Challenge Overview

In this level, the password is stored inside a file whose name begins with a dash (`-`).

A filename beginning with `-` can be interpreted by command-line programs as an **option** rather than a normal filename. Because of this, a command such as `cat -` may not read the intended file correctly.

The key concept is learning how to distinguish **command options** from **filenames**.

---

## Why `-` Causes a Problem

Many Linux commands use a single dash (`-`) to introduce command-line options.

For example:

```bash
command -option
```

A filename such as:

```text
-myfile.txt
```

can therefore be mistaken for an option.

There is also a special convention where `-` can represent **standard input (stdin)** or **standard output (stdout)**. Depending on the command, this can result in unexpected behavior, such as waiting for input.

---

## Methods for Handling Dash-Prefixed Filenames

| Method         | Example                    | Purpose                                                      |
| -------------- | -------------------------- | ------------------------------------------------------------ |
| Relative path  | `cat ./-myfile.txt`        | Makes the filename explicit by adding `./`                   |
| Absolute path  | `cat /path/to/-myfile.txt` | Provides the complete file path                              |
| `--` delimiter | `cat -- -myfile.txt`       | Signals that subsequent arguments are filenames, not options |

---

## Method 1 — Prefix the Filename with `./`

A relative path is often the simplest approach.

Adding `./` changes the argument from:

```text
-myfile.txt
```

to:

```text
./-myfile.txt
```

The command can then recognize it as a path rather than an option.

### Example

```bash
cat ./-myfile.txt
```

The same technique works with other commands.

For example, removing a file:

```bash
rm ./-myfile.txt
```

### Why It Works

The leading character of the argument is now `.` rather than `-`.

```text
-myfile.txt
  ↓
./-myfile.txt
```

The command-line parser therefore receives a path instead of an option-like argument.

---

## Method 2 — Use the `--` Delimiter

Many Unix/Linux commands support `--` as an **end-of-options delimiter**.

Everything appearing after `--` is treated as an argument rather than a command option.

### Example

```bash
cat -- -myfile.txt
```

For file removal:

```bash
rm -- -myfile.txt
```

The important structure is:

```text
command -- filename
```

The `--` tells the command to stop interpreting subsequent arguments as options.

---

## Quick Comparison

| Approach       | Example                    | Main Idea                                        |
| -------------- | -------------------------- | ------------------------------------------------ |
| `./` prefix    | `cat ./-myfile.txt`        | Turn the filename into an explicit relative path |
| `--` delimiter | `cat -- -myfile.txt`       | End option processing before the filename        |
| Absolute path  | `cat /path/to/-myfile.txt` | Refer to the file using its complete path        |

### Recommended Approach

For a simple file in the current directory, the relative-path method is straightforward:

```bash
cat ./-myfile.txt
```

The `--` method is also useful because it demonstrates an important command-line convention:

```bash
cat -- -myfile.txt
```

---

## Key Takeaway

A filename beginning with `-` can be confused with a command-line option.

Two useful techniques are:

```bash
cat ./-filename
```

and:

```bash
cat -- -filename
```

The broader Linux concept is:

> **When a filename begins with `-`, explicitly separate the filename from command options.**
