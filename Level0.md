# Bandit Level 0

## Objective

Bandit Level 0 focuses on connecting to the Bandit server through **SSH (Secure Shell)**.

After a successful login, the environment is ready to proceed from **Level 0 → Level 1**.

---

## 1. Understanding SSH

**SSH (Secure Shell)** is a protocol and command-line tool used to securely log in to a remote machine and execute commands on that machine.

The general SSH command follows this structure:

```bash
ssh -p <port_number> <username>@<IP_or_hostname>
```

### Command breakdown

| Component          | Purpose                             |
| ------------------ | ----------------------------------- |
| `ssh`              | Starts an SSH connection            |
| `-p`               | Specifies the SSH port              |
| `<port_number>`    | Port used by the remote SSH service |
| `<username>`       | Account used for login              |
| `<IP_or_hostname>` | Address of the remote machine       |

---

## 2. Bandit Server Information

The Level 0 information provides the following connection details:

| Parameter | Value                         |
| --------- | ----------------------------- |
| Host      | `bandit.labs.overthewire.org` |
| Port      | `2220`                        |
| Username  | `bandit0`                     |
| Password  | `bandit0`                     |

These details are sufficient to establish the initial SSH connection.

---

## 3. Connecting to the Server

The connection command can be constructed as:

```bash
ssh -p 2220 bandit0@bandit.labs.overthewire.org
```

After executing the command, the SSH client requests the password associated with the `bandit0` account.

The password is provided by the Level 0 challenge itself rather than exposing a password obtained from a later level.

---

## 4. What Happens After Login?

A successful connection opens a shell on the Bandit server.

The prompt changes to indicate that the session is running on the remote machine.

At this point, the initial connection challenge is complete, and the environment is ready for the **Level 1** challenge.

---

## 5. Key Takeaways

| Concept           | Summary                                                |
| ----------------- | ------------------------------------------------------ |
| SSH               | Used for remote login and command execution            |
| `-p`              | Specifies a non-default SSH port                       |
| Hostname          | Identifies the remote Bandit server                    |
| Username          | Identifies the account used for authentication         |
| Password          | Authenticates the account                              |
| Level progression | Successful login provides access to the next challenge |

### Useful command pattern

```bash
ssh -p <port> <username>@<host>
```

For Bandit Level 0, use the information to write the proper ssh command.

This completes the initial server connection without revealing credentials obtained from subsequent Bandit levels.
