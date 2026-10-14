# Bandit Level 25 to Level 26

## Objective

The goal is to access Bandit Level 26 using the private SSH key available in the home directory of Bandit Level 25.

According to the level instructions, the shell assigned to `bandit26` is not `/bin/bash`. The challenge is to identify how the assigned shell works and find a way to access an interactive shell.

## Step 1: Copy the SSH Private Key

The private key is stored in the home directory of `bandit25`. Copy it to the local machine using `scp`.

| Command | Purpose |
|---|---|
| `scp -P 2220 bandit25@bandit.labs.overthewire.org:/home/bandit25/bandit26.sshkey .` | Copies the private key from the remote server to the current local directory. |

**Explanation:**

The `scp` command securely transfers files between systems. The uppercase `-P` specifies the SSH port, while the final `.` indicates the current local directory as the destination.

## Step 2: Attempt to Connect to Bandit Level 26

Use the copied private key to establish an SSH connection.

```bash
ssh -i bandit26.sshkey -p 2220 bandit26@bandit.labs.overthewire.org
```

| Option | Purpose |
|---|---|
| `-i bandit26.sshkey` | Specifies the private key used for authentication. |
| `-p 2220` | Connects through port 2220. |
| `bandit26@bandit.labs.overthewire.org` | Specifies the remote username and server. |

**Expected observation:**

The connection closes almost immediately after authentication. This behavior is related to the shell configured for `bandit26`, rather than necessarily indicating an authentication failure.

## Step 3: Understand the Shell Behavior

The challenge description states that `bandit26` does not use `/bin/bash` as its login shell.

The assigned shell launches a program that displays the level's introductory text and then exits. The key is to understand the behavior of the `more` command and how it can provide access to an editor.

| Command or tool | Role in the process |
|---|---|
| `more` | Displays text one screen at a time. |
| `v` | Opens the configured editor when entered from the interactive `more` prompt. |
| Vim | Provides an editor interface that can be used to start a shell. |

## Step 4: Resize the Terminal

Before reconnecting, reduce the terminal window height so that the displayed text does not fit entirely on the screen.

This encourages `more` to enter its interactive viewing mode instead of immediately displaying all the text and exiting.

Reconnect using the same SSH command:

```bash
ssh -i bandit26.sshkey -p 2220 bandit26@bandit.labs.overthewire.org
```

If the terminal enters the interactive `more` interface, proceed to the next step.

**Note:** The exact terminal dimensions may vary depending on the terminal application and the amount of introductory text displayed.

## Step 5: Open the Editor

While the interactive `more` interface is active, press:

```text
v
```

This should open the configured text editor, which is Vim in the intended workflow.

## Step 6: Configure the Shell in Vim

Inside Vim, press `Esc` to ensure that the editor is in Normal mode.

Enter the following command:

```vim
:set shell=/bin/bash
```

Press `Enter` to apply the setting.

| Vim command | Purpose |
|---|---|
| `:set shell=/bin/bash` | Configures Vim to use `/bin/bash` when launching a shell. |

Next, enter:

```vim
:shell
```

Press `Enter` to launch the configured shell.

## Step 7: Verify the Interactive Shell

If the procedure succeeds, an interactive Bash shell should open within the remote session.

The next objective is to inspect the available files and commands for the remaining steps of Bandit Level 26, without disclosing the level password.

## Summary of the Procedure

| Step | Action | Expected result |
|---|---|---|
| 1 | Copy `bandit26.sshkey` using `scp`. | The private key is available locally. |
| 2 | Connect using SSH and the private key. | The connection closes quickly. |
| 3 | Reduce the terminal height. | The `more` interface can remain interactive. |
| 4 | Reconnect to Bandit Level 26. | The introductory text opens in `more`. |
| 5 | Press `v`. | Vim opens. |
| 6 | Run `:set shell=/bin/bash`. | Vim is configured to use Bash. |
| 7 | Run `:shell`. | An interactive Bash shell opens. |

**Key takeaway:** This level demonstrates how terminal dimensions, a restricted login environment, the `more` pager, and Vim can interact to provide access to a shell.