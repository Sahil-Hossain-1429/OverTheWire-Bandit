# Bandit Level 22: Understanding Cron Jobs

## Objective

The goal of this level is to understand how a scheduled task uses a shell script to generate a filename and copy a password file into `/tmp/`.

The task involves inspecting the cron configuration, understanding the script, and reproducing the command that generates the target filename.

## Step 1: Locate the Cron Configuration

Cron is a time based job scheduler that runs commands automatically at specified intervals.

Navigate to the cron configuration directory:

```bash
cd /etc/cron.d
```

List the available files:

```bash
ls /etc/cron.d
```

Locate the configuration file named `cronjob_bandit23`.

## Step 2: Inspect the Cron Job

Display the configuration:

```bash
cat cronjob_bandit23
```

Expected output:

```bash
@reboot bandit23 /usr/bin/cronjob_bandit23.sh &> /dev/null
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh &> /dev/null
```

The configuration shows that the script runs at system startup and every minute under the `bandit23` account.

## Step 3: Examine the Shell Script

Read the script referenced by the cron configuration:

```bash
cat /usr/bin/cronjob_bandit23.sh
```

Script contents:

```bash
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget
```

### Understanding the Commands

| Command or variable            | Purpose                                                       |
| ------------------------------ | ------------------------------------------------------------- |
| `whoami`                       | Identifies the current user executing the script.             |
| `myname`                       | Stores the username returned by `whoami`.                     |
| `echo I am user $myname`       | Generates a text string containing the username.              |
| `md5sum`                       | Calculates the MD5 hash of the generated string.              |
| `cut -d ' ' -f 1`              | Extracts the hash from the command output.                    |
| `mytarget`                     | Stores the resulting hash, which becomes the target filename. |
| `cat /etc/bandit_pass/$myname` | Reads the password file belonging to the executing user.      |
| `> /tmp/$mytarget`             | Redirects the file contents into a file under `/tmp/`.        |

The important observation is that the filename depends on the username running the script. Since the cron job runs as `bandit23`, the resulting filename can be calculated independently.

## Step 4: Calculate the Target Filename

Execute the following command:

```bash
echo I am user bandit23 | md5sum | cut -d ' ' -f 1
```

The output should be:

```text
8ca319486bfbbc3663ea0fbe81326349
```

This value identifies the temporary file created by the scheduled script.

## Step 5: Read the Generated File

Use the calculated filename to inspect the temporary file:

```bash
cat /tmp/8ca319486bfbbc3663ea0fbe81326349
```

The command displays the contents of the generated file. When the cron job has completed successfully, the file contains the password for the next level.

## Summary

| Step | Action                   | Purpose                                          |
| ---- | ------------------------ | ------------------------------------------------ |
| 1    | Inspect `/etc/cron.d`    | Locate the relevant cron configuration.          |
| 2    | Read `cronjob_bandit23`  | Identify the scheduled script.                   |
| 3    | Examine the shell script | Understand how the target filename is generated. |
| 4    | Calculate the MD5 hash   | Determine the temporary filename.                |
| 5    | Read the file in `/tmp/` | Retrieve the next level's password.              |

**Key takeaway:** Cron jobs can execute scripts automatically under a specific user account. Understanding the execution context and the commands used to construct a filename makes it possible to locate the output of a scheduled task without revealing the password directly in the guide.
