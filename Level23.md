# OverTheWire Bandit Level 23 to Level 24

## Objective

A program runs automatically at regular intervals through `cron`, a time based job scheduler. The task is to inspect the cron configuration, understand the script being executed, and use the script's behavior to retrieve the password for the next level.

## Step 1: Inspect the Cron Configuration

The cron configuration files are stored in `/etc/cron.d/`.

| Command | Purpose |
|---|---|
| `ls /etc/cron.d/` | Lists the available cron configuration files. |
| `cat /etc/cron.d/cronjob_bandit24` | Displays the configuration for the Bandit Level 24 cron job. |

The configuration identifies the script executed by the scheduled job.

## Step 2: Examine the Cron Script

The script is located at `/usr/bin/cronjob_bandit24.sh`.

```bash
cat /usr/bin/cronjob_bandit24.sh
```

The script contains several important operations.

| Script component | Explanation |
|---|---|
| `myname=$(whoami)` | Identifies the account running the script. |
| `cd /var/spool/"$myname"/foo || exit` | Changes to the account's designated working directory. |
| `for i in * .*;` | Iterates over regular and hidden directory entries. |
| `stat --format "%U" "./$i"` | Retrieves the owner of each entry. |
| `[ "${owner}" = "bandit23" ] && [ -f "$i" ]` | Checks whether an entry is a regular file owned by `bandit23`. |
| `timeout -s 9 60 "./$i"` | Executes the qualifying file, with a maximum execution time of 60 seconds. |
| `rm -rf "./$i"` | Removes the entry after processing. |

## Step 3: Understand the Important Behavior

The key behavior is that the scheduled job processes files in:

```bash
/var/spool/bandit24/foo/
```

Files owned by `bandit23` are executed when they meet the script's conditions. After processing, the entries are removed.

This creates an opportunity to execute a custom script through the scheduled job. The script can be prepared in a writable temporary directory and then copied into the designated processing directory.

## Step 4: Prepare a Helper Script

Create a script in `/tmp/`:

```bash
nano /tmp/getpass.sh
```

The helper script should perform two operations:

| Operation | Purpose |
|---|---|
| Read the protected password file for `bandit24` | Attempts to retrieve the credential using the permissions of the scheduled job. |
| Write the result to a temporary output file | Stores the result for later inspection. |

The destination file and its permissions should be chosen so that the result can be read by the intended account.

## Step 5: Set the Script Permissions

Make the helper script executable:

```bash
chmod +x /tmp/getpass.sh
```

This allows the script to run as an executable file when the scheduled job processes it.

## Step 6: Submit the Script to the Processing Directory

Copy the helper script into the directory monitored by the cron job:

```bash
cp /tmp/getpass.sh /var/spool/bandit24/foo/
```

The scheduled job checks this directory at regular intervals. The script must be owned by `bandit23` to satisfy the ownership condition in the cron script.

## Step 7: Inspect the Result

Allow sufficient time for the scheduled job to run and process the helper script. Then inspect the temporary output file created by the script.

```bash
cat /tmp/bandit24_password
```

If the script executes successfully and the output file is accessible, the result should contain the password needed to proceed to Bandit Level 24.

## Key Takeaways

| Concept | What it demonstrates |
|---|---|
| Cron jobs | Automated execution of commands at scheduled intervals. |
| File ownership | Ownership checks can determine which files a script executes. |
| File permissions | Permissions affect access to scripts, protected files, and output. |
| Temporary directories | Temporary storage can be used to transfer results between processes. |
| Script execution | A scheduled job can execute files placed in a monitored directory when its conditions are satisfied. |

**Security lesson:** Automated scripts should carefully validate the files they execute and the permissions of their working directories. Executing files from a writable directory can introduce a privilege escalation risk when the scheduled job runs with greater privileges.