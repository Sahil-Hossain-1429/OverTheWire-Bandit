## Bandit Level 14

### Objective

The password for the next level can be retrieved by submitting the password of the current level to **port 30000 on localhost**.

### Understanding `nc`

`nc` stands for **Netcat**. It is a command line utility that can create network connections between a computer and a specific host and port.

In this level, `nc` is used to connect to a service running on **localhost** through **port 30000**.

### Command

```bash
nc localhost 30000
```

### How the Command Works

| Part        | Meaning                                           |
| ----------- | ------------------------------------------------- |
| `nc`        | Starts Netcat                                     |
| `localhost` | Refers to the current machine                     |
| `30000`     | Specifies the port where the service is listening |

After running the command, the connection remains open and waits for input.

The current level password can then be entered into the connection. If the correct password is provided, the service returns the password required for the next level.

### Simple Example

Consider a service running on port `5000` on the same machine.

```bash
nc localhost 5000
```

This means:

```text
Netcat → connect to localhost → port 5000
```

The Bandit Level 14 command follows the same idea:

```text
Netcat → connect to localhost → port 30000
```

### Key Idea

The important concept in this level is that `nc` can connect directly to a network service using a **host** and **port**.

```bash
nc <host> <port>
```

For this level:

```bash
nc localhost 30000
```

The current level password is submitted through this connection, allowing the service to provide the next level password without exposing it directly in the guide.
