# Bandit Level 11

## Understanding the Challenge

The password is stored in `data.txt`.

The characters in the file have been rotated by **13 positions**. This transformation is commonly known as **ROT13**.

Both lowercase and uppercase letters are affected.

## ROT13 Character Mapping

The alphabet is divided into two groups of 13 characters:


The ROT13 character mapping can be represented as follows:

```text
A B C D E F G H I J K L M
↕ ↕ ↕ ↕ ↕ ↕ ↕ ↕ ↕ ↕ ↕ ↕ ↕
N O P Q R S T U V W X Y Z
```

The same mapping applies to lowercase letters:

```text
a b c d e f g h i j k l m
↕ ↕ ↕ ↕ ↕ ↕ ↕ ↕ ↕ ↕ ↕ ↕ ↕
n o p q r s t u v w x y z
```


## Using `tr`

The `tr` command can translate characters from one set to another.

The general syntax is:

```bash
tr "set1" "set2"
```

Characters from `set1` are translated into the corresponding characters in `set2`.

For ROT13, the command is:

```bash
cat data.txt | tr "a-zA-Z" "n-za-mN-ZA-M"
```

## Understanding the Character Sets

The first set is:

```text
a-zA-Z
```

This represents every lowercase and uppercase letter.

The second set is:

```text
n-za-mN-ZA-M
```

This represents the corresponding ROT13 characters.

For lowercase letters:

| Original | ROT13 |
| -------- | ----- |
| a        | n     |
| b        | o     |
| c        | p     |
| ...      | ...   |
| l        | y     |
| m        | z     |
| n        | a     |
| o        | b     |
| ...      | ...   |
| y        | l     |
| z        | m     |

For uppercase letters:

| Original | ROT13 |
| -------- | ----- |
| A        | N     |
| B        | O     |
| C        | P     |
| ...      | ...   |
| L        | Y     |
| M        | Z     |
| N        | A     |
| O        | B     |
| ...      | ...   |
| Y        | L     |
| Z        | M     |

The important detail is that `tr` performs the translation **position by position**.

For example:

```text
a → n
b → o
c → p
```

After reaching `m`, the mapping continues from the beginning:

```text
n → a
o → b
p → c
```

The same pattern is used for uppercase letters.

## Command Breakdown

| Part             | Purpose                                          |                                      |
| ---------------- | ------------------------------------------------ | ------------------------------------ |
| `cat data.txt`   | Reads the contents of `data.txt`                 |                                      |
| `                | `                                                | Sends the output to the next command |
| `tr`             | Translates characters                            |                                      |
| `"a-zA-Z"`       | Defines lowercase and uppercase input characters |                                      |
| `"n-za-mN-ZA-M"` | Defines their ROT13 equivalents                  |                                      |

The complete command is:

```bash
cat data.txt | tr "a-zA-Z" "n-za-mN-ZA-M"
```

This applies the ROT13 transformation to the contents of `data.txt` and displays the translated result.

## Key Concept

ROT13 is a **substitution cipher** where each alphabetic character is replaced by the character located 13 positions away.

Because the alphabet contains 26 letters, applying ROT13 twice returns the original text:
