# Bandit Level 21

## Objective

A time based job scheduler is running in the background. The goal is to inspect the scheduled job, understand which script it executes, and follow the script to identify where the next level password is written.

## Step 1: Inspect the Cron Directory

Cron configuration files are stored in `/etc/cron.d/`.

Run:

```bash
cd /etc/cron.d/
```

List the available files:

```bash
ls
```

Look for the file associated with Bandit Level 22:

```text
cronjob_bandit22
```

Inspect the file:

```bash
cat cronjob_bandit22
```

The configuration contains entries similar to:

```text
@reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
```

## Understanding the Cron Job

| Part                           | Meaning                                    |
| ------------------------------ | ------------------------------------------ |
| `@reboot`                      | Runs the command when the system starts    |
| `* * * * *`                    | Runs the command every minute              |
| `bandit22`                     | The command runs as the `bandit22` user    |
| `/usr/bin/cronjob_bandit22.sh` | Script executed by the cron job            |
| `&> /dev/null`                 | Redirects standard output and error output |

The important part is the script:

```text
/usr/bin/cronjob_bandit22.sh
```

## Step 2: Inspect the Script

Display the contents of the script:

```bash
cat /usr/bin/cronjob_bandit22.sh
```

The script contains:

```bash
#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```

## Understanding the Script

| Command                                 | Purpose                                       |
| --------------------------------------- | --------------------------------------------- |
| `chmod 644 ...`                         | Changes the permissions of the temporary file |
| `cat /etc/bandit_pass/bandit22`         | Reads the Bandit Level 22 password            |
| `>`                                     | Redirects the output into another file        |
| `/tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv` | Temporary file containing the copied password |

The important relationship is:

```text
/etc/bandit_pass/bandit22
              ↓
cronjob_bandit22.sh
              ↓
/tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```

Since the cron job runs every minute, the script periodically copies the contents of the Bandit Level 22 password file into the temporary file.

## Step 3: Inspect the Temporary File

After the cron job has executed, inspect the destination file:

```bash
cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```

The resulting value is the credential required for the next Bandit level.

## Key Takeaway

This level demonstrates how a cron job can be traced systematically:

| Investigation Stage                   | Command                                     |
| ------------------------------------- | ------------------------------------------- |
| Enter cron configuration directory    | `cd /etc/cron.d/`                           |
| Inspect the Bandit cron configuration | `cat cronjob_bandit22`                      |
| Inspect the executed script           | `cat /usr/bin/cronjob_bandit22.sh`          |
| Inspect the script's output file      | `cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv` |

The main technique is to follow the execution chain from the scheduled job to the script, then from the script to its output file.
