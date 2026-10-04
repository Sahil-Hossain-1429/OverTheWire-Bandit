# Bandit Level 16

## Objective

In this level, the password for the next level can be retrieved by submitting the current level password to a specific port using SSL/TLS encryption.

The important detail is that the connection must use encryption.

## Why `nc` Does Not Work

A standard `nc` command does not provide the required SSL/TLS encryption.

For example:

```bash
nc localhost 30001
```

This approach does not work for the encrypted connection required by this level.

A suitable alternative is `ncat`, which supports SSL/TLS connections.

## Connecting with `ncat`

The following command establishes an SSL/TLS connection to the required port:

```bash
ncat --ssl localhost 30001
```

After the connection is established, enter the password obtained from the current Bandit level.

If the correct password is provided, the service returns the password for the next level.

## Command Summary

| Purpose             | Command                      |
| ------------------- | ---------------------------- |
| Standard connection | `nc localhost 30001`         |
| SSL/TLS connection  | `ncat --ssl localhost 30001` |
| Required tool       | `ncat`                       |
| Encryption          | SSL/TLS                      |
| Target port         | `30001`                      |

## Key Takeaway

The main concept in this level is recognizing when an encrypted network connection is required.

`nc` is not suitable for this task because the required SSL/TLS encryption is unavailable through the command shown above. `ncat` provides the necessary SSL/TLS support through the `--ssl` option.

The password itself is intentionally omitted so that the level remains a guided learning exercise.
