# Bandit Level 13

## Objective

The password for the next level is stored in:

```text
/etc/bandit_pass/bandit14
```

The file can only be read by the `bandit14` user.

Unlike previous levels, the required credential is not provided directly as a password. Instead, Level 13 provides a **private SSH key** that can be used to authenticate as `bandit14`.

The private key is stored as:

```text
~/sshkey.private
```

The main task is therefore to copy the private key from the Bandit server to the local machine and use it to establish a new SSH connection.

---

## Important SSH Restriction

It may seem convenient to connect directly from the `bandit13` session to `bandit14` using the private key.

For example:

```bash
ssh -p 2220 -i sshkey.private bandit14@bandit.labs.overthewire.org
```

However, this connection is rejected because the Bandit SSH server blocks SSH connections originating from the server itself.

An error similar to the following may appear:

```text
!!! You are trying to log into this SSH server from localhost. !!!
!!! Connecting from/to localhost is blocked to conserve resources. !!!
!!! Please log out and log in again, directly from your client machine. !!!

backend: gibson-1

Received disconnect from 127.0.0.1 port 2220:2:
no authentication methods enabled

Disconnected from 127.0.0.1 port 2220
```

The solution is to leave the current Bandit session, copy the private key to the local machine, and then connect to `bandit14` directly from the local machine.

---

## Step 1: Leave the Bandit 13 Session

First, exit the current SSH session:

```bash
exit
```

The terminal should return to the local machine.

---

## Step 2: Copy the Private Key

The `scp` command can copy files between machines over SSH.

Run the following command from the local machine:

```bash
scp -P 2220 bandit13@bandit.labs.overthewire.org:/home/bandit13/sshkey.private .
```

### Command breakdown

| Part                                   | Purpose                                          |
| -------------------------------------- | ------------------------------------------------ |
| `scp`                                  | Securely copies a file over SSH                  |
| `-P 2220`                              | Specifies the SSH port used by Bandit            |
| `bandit13@bandit.labs.overthewire.org` | Remote Bandit account and server                 |
| `/home/bandit13/sshkey.private`        | Location of the private SSH key                  |
| `.`                                    | Copies the file into the current local directory |

After the command completes, the private key should be available locally as:

```text
sshkey.private
```

---

## Step 3: Restrict the Private Key Permissions

SSH requires private keys to have appropriately restricted permissions.

Set the permissions with:

```bash
chmod 600 sshkey.private
```

This gives the file read and write permissions for the owner while removing access for other users.

The permissions can be checked with:

```bash
ls -l sshkey.private
```

A suitable permission pattern should look similar to:

```text
-rw------- 
```

---

## Step 4: Connect as Bandit 14

The private key can now be supplied to SSH with the `-i` option:

```bash
ssh -p 2220 -i sshkey.private bandit14@bandit.labs.overthewire.org
```

### Command breakdown

| Option                                 | Purpose                                            |
| -------------------------------------- | -------------------------------------------------- |
| `ssh`                                  | Starts an SSH connection                           |
| `-p 2220`                              | Connects through the Bandit SSH port               |
| `-i sshkey.private`                    | Uses the downloaded private key for authentication |
| `bandit14@bandit.labs.overthewire.org` | Connects as `bandit14`                             |

If the authentication succeeds, the session will be running as `bandit14`.

The important distinction is that this connection is now initiated from the local machine rather than from inside the `bandit13` server session.

---

## Step 5: Read the Password File

Once authenticated as `bandit14`, the password file can be read with:

```bash
cat /etc/bandit_pass/bandit14
```

The command displays the password required for the next stage.

For security and learning purposes, the actual password is intentionally not included in this guide.

---

## Complete Command Flow

The complete process from the local machine can be summarized as follows:

```bash
scp -P 2220 bandit13@bandit.labs.overthewire.org:/home/bandit13/sshkey.private .

chmod 600 sshkey.private

ssh -p 2220 -i sshkey.private bandit14@bandit.labs.overthewire.org
```

After entering the `bandit14` session:

```bash
cat /etc/bandit_pass/bandit14
```

---

## Key Concept

This level introduces an important distinction between **password based authentication** and **SSH key based authentication**.

| Authentication method | Level 13 situation                 |
| --------------------- | ---------------------------------- |
| Password              | Not provided directly for Level 14 |
| Private SSH key       | Provided as `sshkey.private`       |
| Authentication user   | `bandit14`                         |
| SSH port              | `2220`                             |
| Password location     | `/etc/bandit_pass/bandit14`        |

The private key acts as the authentication credential for the next SSH session. Since the Bandit server prevents direct SSH connections from one Bandit session into another, the key must first be transferred back to the local machine.

## Main Takeaway

The important workflow for this level is:

```text
Bandit 13
    |
    | private SSH key
    v
sshkey.private
    |
    | copy to local machine
    v
Local machine
    |
    | SSH using private key
    v
Bandit 14
    |
    | read password file
    v
Next level password
```

This level demonstrates how an SSH private key can be transferred securely and used as an authentication mechanism for a subsequent SSH connection.
