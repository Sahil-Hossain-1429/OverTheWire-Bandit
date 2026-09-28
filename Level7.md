# Bandit Level 7

| Step | Explanation                                                                                                                |
| ---- | -------------------------------------------------------------------------------------------------------------------------- |
| 1    | A file named `data.txt` is available in the current directory.                                                             |
| 2    | The required password is stored inside this file.                                                                          |
| 3    | The password appears next to the word `millionth`.                                                                         |
| 4    | Since the exact location of the password is not known, a pattern matching command can be used to locate the relevant line. |
| 5    | `grep` is useful for searching text that matches a specified pattern.                                                      |

### Finding the Relevant Line

The following command searches `data.txt` for the word `millionth`:

```bash
grep "millionth" data.txt
```

The command displays the line containing `millionth`. The value appearing next to it is the required password for the next level.

### Key Concept

`grep` is a command line utility commonly used to search for matching text inside files.

General syntax:

```bash
grep "pattern" filename
```

In this level:

```bash
grep "millionth" data.txt
```

The search pattern is `millionth`, and the file being searched is `data.txt`.
