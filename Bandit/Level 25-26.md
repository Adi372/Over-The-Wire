# Bandit Level 25 → Level 26

## Challenge Information

- **Platform:** OverTheWire Bandit
- **Category:** Linux, SSH, Restricted Shell Escape
- **Difficulty:** Intermediate
- **Tools Used:** SSH, SCP, `cat`, `grep`, `file`, `more`, `vi`
- **Objective:** Use the SSH private key provided by `bandit25` to access `bandit26` and escape its restricted login environment.

## 1. Challenge Description

The SSH private key for `bandit26` is available in the home directory of `bandit25`. However, logging in as `bandit26` does not provide a normal interactive shell because its configured login shell is `/usr/bin/showtext`.

The goal is to investigate how this restricted shell works and find a way to obtain an interactive shell as `bandit26`.

## 2. Initial Enumeration

After logging in as `bandit25`, I listed the files in the home directory.

```bash
ls
```

The directory contained:

```text
bandit26.sshkey
```

I checked the file type to confirm that it was an SSH private key.

```bash
file bandit26.sshkey
```

The output identified it as an OpenSSH private key. This key could be used to authenticate as `bandit26`.

Next, I inspected the account information for `bandit26`.

```bash
cat /etc/passwd | grep bandit26
```

The relevant output was:

```text
bandit26:x:11026:11026:bandit level 26:/home/bandit26:/usr/bin/showtext
```

The final field specifies the user's login shell. Instead of `/bin/bash`, `bandit26` uses `/usr/bin/showtext`.

## 3. Analysis and Investigation

To understand the behavior of the restricted shell, I inspected the script.

```bash
cat /usr/bin/showtext
```

The script contained:

```bash
#!/bin/sh

export TERM=linux

exec more ~/text.txt
exit 0
```

Here's what each part does:

- `#!/bin/sh` specifies the interpreter used to run the script.
- `export TERM=linux` sets the terminal type for programs launched by the script.
- `exec more ~/text.txt` replaces the script process with `more`, which displays the contents of `text.txt`.
- `exit 0` is not reached during normal execution because `exec` replaces the current process.

The important detail is that `more` is an interactive pager. If the terminal is small enough that the text cannot fit on one screen, `more` pauses and waits for input.

While `more` is active, pressing `v` can open the displayed file in the `vi` editor. The editor provides a way to launch an external shell, allowing the restricted login environment to be escaped.

## 4. Solution

### Step 1: Copy the SSH private key

I exited the remote session and copied the private key to my local machine.

```bash
scp -P 2220 bandit25@bandit.labs.overthewire.org:/home/bandit25/bandit26.sshkey .
```

Explanation:

- `scp` securely copies files between computers.
- `-P 2220` specifies the SSH server's port. The uppercase `-P` is required by `scp` to set the port.
- The remote path identifies the private key.
- `.` saves the file in the current local directory.

### Step 2: Connect as `bandit26`

I used the private key to authenticate as `bandit26`.

```bash
ssh -i bandit26.sshkey bandit26@bandit.labs.overthewire.org -p 2220
```

The login script launched `more` instead of a regular shell.

Before connecting, I reduced the terminal window height so that the displayed text would not fit on a single screen. This caused `more` to pause and accept interactive input.

### Step 3: Open the file in `vi`

While `more` was active, I pressed:

```text
v
```

This opened the displayed file in the `vi` editor.

### Step 4: Configure and launch Bash

Inside `vi`, I entered:

```vim
:set shell=/bin/bash
```

This sets the shell that `vi` uses when executing shell commands.

I then entered:

```vim
:shell
```

This launched an interactive Bash shell.

### Step 5: Verify access

To confirm that the shell belonged to `bandit26`, I ran:

```bash
whoami
```

The output was:

```text
bandit26
```

This confirmed that I had obtained an interactive shell as `bandit26`.

## Mistakes and Corrections

**Using the wrong SCP option**

Initially, using `scp -p 2220` caused the command to interpret `2220` as a file or path argument rather than the SSH port.

- `-p` preserves file permissions and timestamps.
- `-P` specifies the remote SSH port.

The correct command is:

```bash
scp -P 2220 bandit25@bandit.labs.overthewire.org:/home/bandit25/bandit26.sshkey .
```

## 5. What I Learned

- **SSH private keys:** Authenticate users through public-key authentication.
- **`/etc/passwd`:** Stores account information, including the configured login shell.
- **Restricted login shells:** Can launch a specific program instead of providing a normal command-line shell.
- **`exec`:** Replaces the current process with another program.
- **`more`:** Displays text interactively and can pause when the terminal cannot fit all the content.
- **Terminal dimensions:** Affect how terminal applications display information and handle interactive input.
- **`vi` shell escape:** The editor can launch an external shell through commands such as `:shell`.
- **Security boundaries:** Restricting a user's login shell does not necessarily isolate them from the functionality exposed by the programs that shell launches.

## 6. References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [OpenSSH Manual Pages](https://man.openbsd.org/ssh)
- [`more` Manual Page](https://man7.org/linux/man-pages/man1/more.1.html)
- [`vi` Manual Page](https://man7.org/linux/man-pages/man1/vi.1p.html)

## Key Takeaway

This level demonstrates how understanding a restricted environment can reveal an unintended path to a shell. By examining `/usr/bin/showtext`, using the interactive behavior of `more`, and invoking Bash from `vi`, I obtained an interactive shell as `bandit26`.

The key lesson is to investigate how a restriction is implemented rather than assuming that a non-standard login shell provides complete isolation.