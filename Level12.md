# Bandit Level 12

## Simplifying all the above steps

The main idea of this level is simple:

`data.txt` is a **hexdump** of a file that has been compressed multiple times using different compression and archive formats.

The process is therefore:

| What is found         | What to do                                      |
| --------------------- | ----------------------------------------------- |
| Hexdump               | Convert it back to a binary file using `xxd -r` |
| gzip compressed data  | Rename it with `.gz`, then use `gunzip`         |
| bzip2 compressed data | Use `bunzip2`                                   |
| POSIX tar archive     | Use `tar -xf`                                   |
| ASCII text            | The extraction process is complete              |

The important rule is:

> **After every extraction, always use `file` to identify the type of the newly created file.**

The compression format determines the next command.

The files are extracted in a temporary directory because the current directory does not provide permission to create the additional files and directories required during the process.

The complete process can be viewed as:

```text
data.txt
   ↓
hexdump → convert with xxd
   ↓
gzip → gunzip
   ↓
bzip2 → bunzip2
   ↓
gzip → gunzip
   ↓
tar → tar -xf
   ↓
tar → tar -xf
   ↓
bzip2 → bunzip2
   ↓
tar → tar -xf
   ↓
gzip → gunzip
   ↓
ASCII text
```

### The rule for every step

Each time a new file is produced:

```bash
file <new_file>
```

Then identify the file type and select the appropriate command.

If the file is gzip compressed:

```bash
mv <file> <file>.gz
gunzip <file>.gz
```

If the file is bzip2 compressed:

```bash
bunzip2 <file>
```

If the file is a tar archive:

```bash
tar -xf <file>
```

If the file is an ASCII text file, the extraction chain has reached its end.

Renaming and removing files are **substeps**, not separate steps. A new step begins only when an extraction produces the next file.

---

# Step 1: Create a temporary directory

The current directory does not provide permission to create the files needed during extraction.

Create a temporary directory:

```bash
mkdir /tmp/my_dir
```

Move into the temporary directory:

```bash
cd /tmp/my_dir
```

Copy `data.txt` into the temporary directory:

```bash
cp ~/data/data.txt .
```

Rename the copied hexdump to `data`:

```bash
mv data.txt data
```

The file is now ready to be converted from a hexdump back into binary data.

---

# Step 2: Convert the hexdump into a binary file

The current file is a hexdump, so it must first be converted back into its original binary form.

Run:

```bash
xxd -r data > xxd2Bin
```

A new file named `xxd2Bin` is created.

Check its type:

```bash
file xxd2Bin
```

The result identifies the first compression format.

In this case, the result is:

```text
xxd2Bin: gzip compressed data, was "data2.bin"
```

The next extraction therefore uses gzip.

---

# Step 3: Extract the gzip file

The file needs the `.gz` extension before using `gunzip`.

Rename the file:

```bash
mv xxd2Bin xxd2Bin.gz
```

Extract it:

```bash
gunzip xxd2Bin.gz
```

A new file named `xxd2Bin` is produced.

Check the new file:

```bash
file xxd2Bin
```

The result is:

```text
xxd2Bin: bzip2 compressed data, block size = 900k
```

The next compression format is bzip2.

---

# Step 4: Extract the bzip2 file

Extract the file using:

```bash
bunzip2 xxd2Bin
```

A new file named:

```text
xxd2Bin.out
```

is produced.

Check its type:

```bash
file xxd2Bin.out
```

The result is:

```text
xxd2Bin.out: gzip compressed data
```

The next compression format is gzip.

---

# Step 5: Extract the gzip file

The file needs the `.gz` extension before using `gunzip`.

Rename it:

```bash
mv xxd2Bin.out xxd2Bin.gz
```

Extract it:

```bash
gunzip xxd2Bin.gz
```

A new file named `xxd2Bin` is produced.

Check its type:

```bash
file xxd2Bin
```

The result is:

```text
xxd2Bin: POSIX tar archive (GNU)
```

The next format is a tar archive.

---

# Step 6: Extract the tar archive

Extract the archive:

```bash
tar -xf xxd2Bin
```

A new file named:

```text
data5.bin
```

is produced.

The old files can now be removed to keep the directory easier to understand:

```bash
rm data xxd2Bin
```

Check the newly created file:

```bash
file data5.bin
```

The result is:

```text
data5.bin: POSIX tar archive (GNU)
```

Another tar archive needs to be extracted.

---

# Step 7: Extract the second tar archive

Extract the archive:

```bash
tar -xf data5.bin
```

A new file named:

```text
data6.bin
```

is produced.

Check the new file:

```bash
file data6.bin
```

The result is:

```text
data6.bin: bzip2 compressed data, block size = 900k
```

The next format is bzip2.

---

# Step 8: Extract the bzip2 file

Extract the file:

```bash
bunzip2 data6.bin
```

A new file named:

```text
data6.bin.out
```

is produced.

Check the new file:

```bash
file data6.bin.out
```

The result is:

```text
data6.bin.out: POSIX tar archive (GNU)
```

The next format is a tar archive.

---

# Step 9: Extract the tar archive

Extract the archive:

```bash
tar -xf data6.bin.out
```

A new file named:

```text
data8.bin
```

is produced.

Check the new file:

```bash
file data8.bin
```

The result is:

```text
data8.bin: gzip compressed data, was "data9.bin"
```

The next format is gzip.

---

# Step 10: Extract the gzip file

The file needs the `.gz` extension before using `gunzip`.

Rename it:

```bash
mv data8.bin data8.gz
```

Extract it:

```bash
gunzip data8.gz
```

A new file named:

```text
data8
```

is produced.

Check the final file:

```bash
file data8
```

The result is:

```text
data8: ASCII text
```

The repeated extraction process is now complete.

The resulting ASCII text file contains the information required for the next level.

---

# Quick Reference

The entire process follows this pattern:

| Step | File type   | Command                                              |
| ---- | ----------- | ---------------------------------------------------- |
| 1    | Hexdump     | `xxd -r data > xxd2Bin`                              |
| 2    | gzip        | `mv xxd2Bin xxd2Bin.gz` then `gunzip xxd2Bin.gz`     |
| 3    | bzip2       | `bunzip2 xxd2Bin`                                    |
| 4    | gzip        | `mv xxd2Bin.out xxd2Bin.gz` then `gunzip xxd2Bin.gz` |
| 5    | tar archive | `tar -xf xxd2Bin`                                    |
| 6    | tar archive | `tar -xf data5.bin`                                  |
| 7    | bzip2       | `bunzip2 data6.bin`                                  |
| 8    | tar archive | `tar -xf data6.bin.out`                              |
| 9    | gzip        | `mv data8.bin data8.gz` then `gunzip data8.gz`       |
| 10   | ASCII text  | Extraction complete                                  |

## The most important command

Whenever a new file appears, check it before deciding what to do next:

```bash
file <filename>
```

This makes the process systematic rather than requiring the compression sequence to be memorized.

The general workflow is:

```text
Extract
   ↓
New file appears
   ↓
file <filename>
   ↓
Identify the format
   ↓
Use the appropriate extraction command
   ↓
New file appears
   ↓
Repeat
```

Continue until `file` reports:

```text
ASCII text
```

At that point, the repeated compression and extraction chain has ended.
