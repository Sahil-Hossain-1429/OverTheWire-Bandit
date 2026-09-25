# Bandit Level 4

## Objective

The password for the next level is stored inside a **human readable file** in the `inhere` directory.

The main challenge is identifying which file contains readable text without directly revealing the password.

## 1. Enter the `inhere` Directory

The `cd` command changes the current working directory.

```bash
cd inhere
```

The contents of the directory can then be inspected with:

```bash
ls
```

However, `ls` only displays the filenames. It does not indicate whether a file contains human readable text.

## 2. Why `ls -lh` Is Not Enough

The `ls -lh` command displays file information using human readable units for file sizes.

```bash
ls -lh
```

In this level, file size is not the information that matters. The important question is the **type of each file**.

A human readable file generally contains plain text, such as ASCII or UTF 8 encoded text.

## 3. Check the File Type

The `file` command identifies the type of a file.

For example:

```bash
file filename
```

A result such as:

```text
filename: ASCII text
```

indicates that the file contains readable text.

The problem is that `file` normally needs a filename as an argument. Checking every file individually would be inefficient when the directory contains many files.

This is where `find` becomes useful.

## 4. Find Files Recursively

The `find` command can search through a directory and its contents.

```bash
find inhere/
```

This displays the files located inside `inhere` and its subdirectories.

The output can then be passed to `file` so that the type of each discovered file can be inspected.

## 5. Combine `find` and `file`

The following command runs `file` against the files discovered by `find`:

```bash
find inhere/ -exec file {} +
```

The important parts of the command are:

<table>
<tr>
<th>Part</th>
<th>Purpose</th>
</tr>
<tr>
<td><code>find inhere/</code></td>
<td>Searches recursively inside the <code>inhere</code> directory.</td>
</tr>
<tr>
<td><code>-exec</code></td>
<td>Runs another command for each set of files found.</td>
</tr>
<tr>
<td><code>file</code></td>
<td>Identifies the type of each file.</td>
</tr>
<tr>
<td><code>{}</code></td>
<td>Represents the filenames discovered by <code>find</code>.</td>
</tr>
<tr>
<td><code>+</code></td>
<td>Passes multiple filenames to <code>file</code> at once.</td>
</tr>
</table>

The result will contain information similar to (**this is an example**):

```text
inhere/file01: data
inhere/file02: ASCII text
inhere/file03: data
```

The exact filenames and output depend on the environment.

## 6. Understand `-exec`

The `-exec` action needs to know where the command ends.

It can be terminated using either `;` or `+`.

For example:

```bash
find inhere/ -exec file {} \;
```

or:

```bash
find inhere/ -exec file {} +
```

Both forms execute `file` on the discovered files, but they handle the arguments differently.

The `+` form groups multiple filenames together before executing the command, making it a useful choice for this situation.

## 7. Identify the Human Readable File

After running:

```bash
find inhere/ -exec file {} +
```

the output can be inspected for entries containing descriptions such as:

```text
ASCII text
```

or another plain text format.

`grep` can help filter the output:

```bash
find inhere/ -exec file {} + | grep ASCII
```

This reduces the output to entries whose file type contains `ASCII`.

The resulting filename can then be inspected with an appropriate command such as:

```bash
cat filename
```

The important concept in this level is the combination of:

```text
find → locate files
file → identify file types
grep → filter relevant output
cat → read the identified text file
```

This approach solves the level without relying on the filename or revealing the password directly.
