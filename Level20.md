# Bandit Level 20

## Objective

The level requires establishing a TCP connection with the `suconnect` program. When the connection is established successfully, the program provides the password for the next level.

## Step 1: Start a TCP Listener

Open a second terminal and start a Netcat listener on port `12345`.

```bash
nc -lv 12345
```

The listener waits for an incoming TCP connection.

## Step 2: Run `suconnect`

In the first terminal, navigate to the home directory and execute the provided setuid binary with the same port number.

```bash
./suconnect 12345
```

The program attempts to establish a TCP connection to the listener running on port `12345`.

## Step 3: Provide the Previous Level Password

After the connection is established, return to the terminal running Netcat.

Paste the password obtained from the previous Bandit level into the Netcat session.

```text
[previous level password]
```

The password should remain hidden in a public writeup.

## How the Process Works

| Component               | Purpose                                  |
| ----------------------- | ---------------------------------------- |
| `nc -lv 12345`          | Starts a TCP listener on port `12345`    |
| `./suconnect 12345`     | Connects to the TCP listener             |
| Previous level password | Sent through the established connection  |
| `suconnect`             | Verifies the received password           |
| Successful verification | Produces the password for the next level |

The important concept in this level is the interaction between a TCP listener and a client connection. Netcat acts as the listener, while `suconnect` acts as the connecting program.

## Command Summary

**Terminal 1**

```bash
nc -lv 12345
```

**Terminal 2**

```bash
./suconnect 12345
```

Once the TCP connection is established, the previous level password can be entered into the Netcat session. The resulting output contains the credential required to continue to the next Bandit level.
