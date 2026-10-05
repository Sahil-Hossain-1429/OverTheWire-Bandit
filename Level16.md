# Bandit Level 16

## Level Objective

In this level, a service is available somewhere within the port range **31000 to 32000**.

The task is to identify the correct port, connect to the service using SSL/TLS, and retrieve an **OpenSSH private key**.

The private key can then be used to authenticate to the next level.

## Step 1: Scan the Port Range

Start by scanning ports from `31000` through `32000`:

```bash
nmap localhost -p 31000-32000
```

The scan displays the ports that are available within the specified range.

| Command                         | Purpose                                              |
| ------------------------------- | ---------------------------------------------------- |
| `nmap localhost -p 31000-32000` | Scans ports 31000 through 32000 on the local machine |
| `localhost`                     | Refers to the current Bandit server                  |
| `-p 31000-32000`                | Specifies the port range                             |

Several ports may appear in the scan results. The next step is to test the available services.

## Step 2: Check the Available Ports

The service containing the required information uses SSL/TLS.

Each candidate port can be tested with:

```bash
openssl s_client -connect localhost:<port> -quiet
```

Replace `<port>` with a port identified by the `nmap` scan.

After connecting, provide the password from the current Bandit level.

The correct service returns an **OpenSSH private key** beginning with:

```text
-----BEGIN OPENSSH PRIVATE KEY-----
```

and ending with:

```text
-----END OPENSSH PRIVATE KEY-----
```

The private key is required for authentication to the next level.

## Step 3: Create a Temporary Directory

Create a directory inside `/tmp` for storing the private key:

```bash
mkdir /tmp/my_dir
```

A temporary directory is useful because `/tmp` is writable and provides a convenient location for files created during the level.

## Step 4: Save the Private Key

The private key can be extracted directly from the SSL/TLS service and saved into a file:

```bash
openssl s_client -connect localhost:<port> -quiet 2>/dev/null | sed -n '/-----BEGIN OPENSSH PRIVATE KEY-----/,/-----END OPENSSH PRIVATE KEY-----/p' > /tmp/my_dir/bandit17.key
```

The current level password is requested during the connection.

The extracted private key is then saved as:

```text
/tmp/my_dir/bandit17.key
```

The `sed` command extracts only the section between the beginning and ending private key markers.

| Part                                  | Purpose                                   |
| ------------------------------------- | ----------------------------------------- |
| `openssl s_client`                    | Establishes an SSL/TLS connection         |
| `-connect localhost:<port>`           | Connects to the selected service          |
| `-quiet`                              | Reduces connection output                 |
| `2>/dev/null`                         | Suppresses error output                   |
| `sed -n`                              | Processes and prints the required section |
| `-----BEGIN OPENSSH PRIVATE KEY-----` | Marks the beginning of the key            |
| `-----END OPENSSH PRIVATE KEY-----`   | Marks the end of the key                  |
| `>`                                   | Redirects the extracted key into a file   |
| `/tmp/my_dir/bandit17.key`            | Destination file                          |

## Step 5: Copy the Private Key

After the key has been saved on the Bandit server, copy it to the local machine:

```bash
scp -P 2220 bandit16@bandit.labs.overthewire.org:/tmp/my_dir/bandit17.key .
```

The final `.` means the file is copied into the current local directory.

The resulting file is:

```text
bandit17.key
```

## Step 6: Use the Private Key

The private key can now be used to authenticate as `bandit17`:

```bash
ssh -i bandit17.key -p 2220 bandit17@bandit.labs.overthewire.org
```

| Option                                 | Purpose                                      |
| -------------------------------------- | -------------------------------------------- |
| `-i bandit17.key`                      | Specifies the private key for authentication |
| `-p 2220`                              | Specifies the Bandit SSH port                |
| `bandit17@bandit.labs.overthewire.org` | Connects as the next Bandit user             |

## Command Summary

| Step                           | Command                                                                                                                                                                           |
| ------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Scan ports                     | `nmap localhost -p 31000-32000`                                                                                                                                                   |
| Test a candidate port          | `openssl s_client -connect localhost:<port> -quiet`                                                                                                                               |
| Create temporary directory     | `mkdir /tmp/my_dir`                                                                                                                                                               |
| Extract the private key        | `openssl s_client -connect localhost:<port> -quiet 2>/dev/null \| sed -n '/-----BEGIN OPENSSH PRIVATE KEY-----/,/-----END OPENSSH PRIVATE KEY-----/p' > /tmp/my_dir/bandit17.key` |
| Copy the key locally           | `scp -P 2220 bandit16@bandit.labs.overthewire.org:/tmp/my_dir/bandit17.key .`                                                                                                     |
| Authenticate to the next level | `ssh -i bandit17.key -p 2220 bandit17@bandit.labs.overthewire.org`                                                                                                                |

## Key Concepts

| Concept                | Explanation                                                 |
| ---------------------- | ----------------------------------------------------------- |
| Port scanning          | Identifies available services within a specified port range |
| SSL/TLS                | Provides an encrypted connection to the target service      |
| `openssl s_client`     | Allows interaction with an SSL/TLS service                  |
| OpenSSH private key    | Provides key based SSH authentication                       |
| `sed`                  | Extracts the private key section from the service output    |
| `scp`                  | Transfers the private key from the Bandit server            |
| SSH key authentication | Allows login without entering the account password          |
| `/tmp`                 | Provides a writable temporary location for the key          |

The main objective is to identify the connect SSL/TLS service and extract the private key without exposing the actual credential. The key then serves as the authentication method for Bandit Level 17.
