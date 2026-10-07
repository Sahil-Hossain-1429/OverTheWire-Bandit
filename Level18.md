# Bandit Level 18

## Objective

The password for the next level is stored in a file named `readme` in the home directory.

However, `.bashrc` has been modified so that an SSH login to Level 18 immediately logs out.

| Situation                     | Result                                            |
| ----------------------------- | ------------------------------------------------- |
| Normal SSH login              | The session immediately logs out                  |
| SSH connection with a command | The command can execute before the session closes |
| Required file                 | `readme`                                          |
| File location                 | Home directory                                    |

## Understanding the Problem

A normal SSH connection opens an interactive shell on the remote server.

```bash
ssh bandit18@bandit.labs.overthewire.org -p 2220
```

For this level, the interactive shell is not useful because the modified `.bashrc` causes the session to log out.

Instead, SSH can be given a command to execute directly on the remote server.

## Remote Command Execution

The general structure is:

```text
ssh USER@SERVER -p PORT COMMAND
```

| Part          | Meaning                                         |
| ------------- | ----------------------------------------------- |
| `ssh`         | Establishes an SSH connection                   |
| `USER@SERVER` | Specifies the remote account and server         |
| `-p 2220`     | Specifies the SSH port                          |
| `COMMAND`     | Runs the specified command on the remote server |

For this level:

```bash
ssh bandit18@bandit.labs.overthewire.org -p 2220 cat readme
```

The important part is `cat readme`.

It instructs the remote server to execute `cat` on the `readme` file rather than opening an interactive shell.

## Local `cat` vs Remote `cat`

The location where `cat` runs depends on the command structure.

| Command                              | Where `cat` runs | Purpose                             |
| ------------------------------------ | ---------------- | ----------------------------------- |
| `cat readme`                         | Local machine    | Reads a local file                  |
| `ssh user@server -p 2220 cat readme` | Remote server    | Reads `readme` on the remote server |

A useful way to visualize the remote command is:

```text
ssh USER@SERVER -p PORT COMMAND
│   │             │       │
│   │             │       └── Execute COMMAND on the remote machine
│   │             └─────────── Connect through the specified SSH port
│   └───────────────────────── Connect as the specified user
└───────────────────────────── Start an SSH connection
```

## Recommended Command

Run:

```bash
ssh bandit18@bandit.labs.overthewire.org -p 2220 cat readme
```

The command executes `cat readme` on the Bandit server, allowing the contents of the required file to be displayed without relying on an interactive shell.

## Key Concept

The main lesson from this level is that SSH does not have to start an interactive shell.

A command can be supplied after the connection details:

```bash
ssh USER@SERVER -p PORT COMMAND
```

This makes it possible to execute a specific command on the remote machine even when an interactive login session is immediately terminated.
