# Bandit Level 8

## Understanding the Task

The password is stored in `data.txt`.

The important condition is that the password appears **only once** in the file. The goal is therefore to identify the line that occurs exactly once.

A useful command for this task is:

```bash
uniq -u
```

The `-u` option reports only lines that occur once.

However, there is an important detail about how `uniq` works.

## Why `uniq -u` Alone Does Not Work

The `uniq` command checks **adjacent lines**. It does not search the entire file and count every occurrence of each line.

Consider this example:

```text
apple
banana
apple
banana
```

Running:

```bash
uniq
```

produces:

```text
apple
banana
apple
banana
```

Nothing is removed because no identical lines are directly next to each other.

Now consider a sorted version:

```text
apple
apple
banana
banana
```

Running:

```bash
uniq
```

produces:

```text
apple
banana
```

The repeated lines are removed because identical lines are adjacent.

The same principle applies to:

```bash
uniq -u
```

For example:

```text
apple
banana
apple
orange
```

Running:

```bash
uniq -u
```

keeps both occurrences of `apple` because they are separated by `banana`.

This is why the data needs to be sorted first.

## Preparing the Data

The `sort` command places identical lines next to each other.

For example:

```text
apple
banana
apple
orange
```

After:

```bash
sort data.txt
```

the structure becomes:

```text
apple
apple
banana
orange
```

Now `uniq -u` can correctly identify the line that occurs only once.

## Final Command

The two commands can be combined with a pipe:

```bash
sort data.txt | uniq -u
```

Here is the process:

| Command         | Purpose                                     |                                             |
| --------------- | ------------------------------------------- | ------------------------------------------- |
| `sort data.txt` | Arranges identical lines next to each other |                                             |
| `uniq -u`       | Displays only lines that occur once         |                                             |

The resulting output identifies the unique line without exposing the password directly in the guide.

## Key Concept

The important detail is that `uniq` works with **adjacent duplicate lines**.

Therefore:

```bash
uniq -u
```

should be used after sorting when the objective is to find a line that occurs only once throughout the file:

```bash
sort data.txt | uniq -u
```
