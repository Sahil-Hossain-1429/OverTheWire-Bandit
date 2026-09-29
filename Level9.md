# Bandit Level 9

## Understanding the Challenge

The password is stored inside `data.txt`, but the file contains a large amount of non human readable data.

Among the unreadable content, a small number of human readable strings contain several `=` characters. The required information can therefore be located by extracting readable strings and filtering the results.

## Command

```bash
strings data.txt | grep ===
```

## Command Breakdown

| Command            | Purpose                                              |
| ------------------ | ---------------------------------------------------- |
| `strings data.txt` | Extracts human readable strings from `data.txt`      |
| `\|`               | Passes the output of `strings` to the next command   |
| `grep ===`         | Displays only the lines containing the `===` pattern |

## What to Look For

The command filters the large amount of data and displays the readable lines containing the relevant `=` pattern.

The resulting output provides the information needed to proceed to the next Bandit level without directly exposing the password in the guide.
