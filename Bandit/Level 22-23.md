# Bandit Level 22 → Level 23

## Challenge Information

- **Platform:** OverTheWire Bandit
- **Category:** Linux, Cron Jobs, Bash Scripting, MD5 Hashing
- **Difficulty:** Beginner
- **Tools Used:** `cat`, `file`, `whoami`, `echo`, `md5sum`, `cut`, `ssh`
- **Objective:** Analyze the scheduled cron script, understand how it generates a temporary filename, and retrieve the password for `bandit23`.

## 1. Challenge Description

A program runs automatically at regular intervals using cron. The goal is to inspect the configuration in `/etc/cron.d/`, find the script executed as `bandit23`, and understand how it stores the next password in a temporary file.

The key is to reproduce the filename-generation logic used by the script.

## 2. Inspecting the Cron Configuration

I navigated to the cron configuration directory:

```bash
cd /etc/cron.d
```

I listed the files and checked their types:

```bash
file ./*
```

I then read the configuration for `bandit23`:

```bash
cat cronjob_bandit23
```

The output was:

```cron
@reboot bandit23 /usr/bin/cronjob_bandit23.sh &> /dev/null
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh &> /dev/null
```

This showed that `/usr/bin/cronjob_bandit23.sh` runs as `bandit23` at startup and every minute.

## 3. Analyzing the Script

I examined the script:

```bash
cat /usr/bin/cronjob_bandit23.sh
```

It contained:

```bash
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget
```

### Understanding each command

**Step 1: Identify the executing user**

```bash
myname=$(whoami)
```

`whoami` returns the effective username running the script. Because cron runs this job as `bandit23`, the variable `myname` becomes `bandit23`.

**Step 2: Generate the temporary filename**

```bash
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)
```

This command performs three operations:

- `echo I am user $myname` produces the text `I am user bandit23`.
- `md5sum` calculates the MD5 hash of that text.
- `cut -d ' ' -f 1` extracts the first field, which contains the hash.

The resulting hash is used as the filename inside `/tmp/`.

**Step 3: Copy the password**

```bash
cat /etc/bandit_pass/$myname > /tmp/$mytarget
```

Because `myname` is `bandit23`, the script reads `/etc/bandit_pass/bandit23` and writes its contents to the generated temporary file.

## 4. Reproducing the Filename

I checked my current username:

```bash
whoami
```

The output was:

```text
bandit22
```

The script uses the username of the user running the cron job, not the username of the person manually executing it. Therefore, I needed to calculate the hash using `bandit23`, not `bandit22`.

I first tested a different input:

```bash
echo I am bandit23 | md5sum
```

This produced a different hash because the input text did not match the script's exact string.

I then reproduced the script's input and extraction logic:

```bash
echo I am user bandit23 | md5sum | cut -d ' ' -f 1
```

The result was:

```text
8ca319486bfbbc3663ea0fbe81326349
```

This gave me the filename that the script uses for the password.

## 5. Retrieving the Next Password

I read the generated temporary file:

```bash
cat /tmp/8ca319486bfbbc3663ea0fbe81326349
```

The terminal displayed the password for `bandit23`.

The actual password is intentionally omitted from this write-up.

## 6. Logging In to bandit23

After retrieving the password, I exited the current session and connected to the next account:

```bash
ssh -p 2220 bandit23@bandit.labs.overthewire.org
```

I entered the retrieved password when prompted.

## 7. Key Learnings

- **Cron:** Runs scheduled commands automatically.
- **`/etc/cron.d/`:** Contains system cron job configuration files.
- **`whoami`:** Displays the effective username of the executing process.
- **Bash variables:** Store values for use later in a script.
- **Command substitution (`$(...)`):** Captures a command's output and assigns or uses it as a value.
- **`md5sum`:** Calculates an MD5 hash of input data.
- **`cut -d ' ' -f 1`:** Splits input using a space delimiter and extracts the first field.
- **Output redirection (`>`):** Writes command output to a file, replacing existing contents.
- **Exact input matters:** Even a small change in the text supplied to a hash function produces a different hash.
- **Script analysis:** Reading a script helps reveal its execution logic and identify how it handles files and credentials.

## 8. References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [`crontab` manual](https://man7.org/linux/man-pages/man5/crontab.5.html)
- [`md5sum` documentation](https://www.gnu.org/software/coreutils/manual/html_node/md5sum-invocation.html)
- [`cut` documentation](https://www.gnu.org/software/coreutils/manual/html_node/cut-invocation.html)
- [Bash manual](https://www.gnu.org/software/bash/manual/)

**Security note:** MD5 is not suitable for modern security-sensitive cryptographic applications. In this challenge, it is used only to generate a predictable temporary filename. Never publish actual passwords or private credentials in a public repository.