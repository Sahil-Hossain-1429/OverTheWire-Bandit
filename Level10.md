# Bandit Level 10

| Item                 | Details                                  |
| -------------------- | ---------------------------------------- |
| **Objective**        | Locate the password stored in `data.txt` |
| **File format**      | Base64 encoded data                      |
| **Required command** | `base64 -d data.txt`                     |

## Step 1: Inspect the File

The password is stored inside `data.txt`, but the contents are encoded using **Base64**.

## Step 2: Decode the Contents

Base64 encoded data can be decoded with the `base64` command.

```bash
base64 -d data.txt
```

The command decodes the contents of `data.txt` and displays the resulting text in the terminal.

## Step 3: Identify the Result

The decoded output contains the information required to proceed to the next Bandit level.

The password itself is not included in this guide.

## Command Summary

| Purpose           | Command              |
| ----------------- | -------------------- |
| Decode `data.txt` | `base64 -d data.txt` |

### Key Concept

**Base64** is an encoding format, not encryption. The `base64 -d` option performs the decoding operation and converts the encoded content back into readable text.
