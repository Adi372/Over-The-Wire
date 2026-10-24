# Bandit Level 21 → Level 22

## Challenge Information

- **Platform:** OverTheWire Bandit
- **Category:** Linux, Cron Jobs, File Permissions
- **Difficulty:** Beginner
- **Tools Used:** `ls`, `cd`, `file`, `cat`, `cron`
- **Objective:** Find the cron job responsible for generating the password file for `bandit22` and retrieve the password.

## 1. Challenge Description

A program runs automatically at regular intervals using **cron**, the time-based job scheduler in Linux.

The task is to inspect the configuration files in `/etc/cron.d/`, identify the job running as `bandit22`, and understand what its script does.

## 2. Exploring the Cron Configuration Directory

I listed the contents of my home directory and navigated to `/etc/cron.d/`:

```bash
ls
cd /etc/cron.d
ls
```

The directory contained several cron configuration files, including:

```text
cronjob_bandit22
cronjob_bandit23
cronjob_bandit24
```

I used `file` to inspect the files:

```bash
file ./*
```

This helped identify which entries were readable text files. Some files could not be read because of their permissions.

## 3. Inspecting the Cron Job

I opened the configuration for `bandit22`:

```bash
cat cronjob_bandit22
```

The output was:

```cron
@reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
```

### Understanding the configuration

- `@reboot` runs the job when the system starts.
- `* * * * *` schedules the job to run every minute.
- `bandit22` specifies the user under whose account the command runs.
- `/usr/bin/cronjob_bandit22.sh` is the script being executed.
- `&> /dev/null` redirects standard output and standard error to `/dev/null`, suppressing the script's output.

The configuration showed that the script runs automatically as `bandit22` every minute.

## 4. Inspecting the Script

Next, I examined the script itself:

```bash
cat /usr/bin/cronjob_bandit22.sh
```

The script contained:

```bash
#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```

### Understanding the script

**First command:**

```bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```

This sets the file's permissions so that the owner can read and write it, while group members and others can read it.

**Second command:**

```bash
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```

This reads the password file for `bandit22` and redirects its contents into a temporary file.

The `>` operator creates the destination file if it does not exist, or overwrites its contents if it does.

The script therefore copies the password into a temporary file with permissions that allow other users to read it.

## 5. Retrieving the Password

After examining the script, I read the temporary file:

```bash
cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```

The terminal displayed the password for `bandit22`.

I used that password to log in to the next level. The actual password is intentionally omitted from this write-up.

## 6. Connecting to bandit22

After leaving the `bandit21` session, I connected to `bandit22`:

```bash
ssh -p 2220 bandit22@bandit.labs.overthewire.org
```

I entered the password retrieved from the temporary file when prompted.

## 7. Key Learnings

- **Cron:** A Linux service that schedules commands to run automatically.
- **`/etc/cron.d/`:** A directory containing system cron job configuration files.
- **`* * * * *`:** A cron schedule that runs a job every minute.
- **`@reboot`:** Runs a scheduled command when the system starts.
- **`cat`:** Reads and displays file contents.
- **Output redirection (`>`):** Writes command output to a file, replacing existing contents.
- **`chmod 644`:** Gives the owner read/write access and gives group members and others read access.
- **`/dev/null`:** Discards data redirected to it.
- **Scheduled scripts:** Automated jobs can create or update files without manual interaction.

## 8. References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [`cron` manual](https://man7.org/linux/man-pages/man8/cron.8.html)
- [`crontab` file format — `man 5 crontab`](https://man7.org/linux/man-pages/man5/crontab.5.html)
- [`chmod` manual](https://man7.org/linux/man-pages/man1/chmod.1.html)

**Security note:** Automated scripts can accidentally expose sensitive information through files with overly permissive access. Never publish passwords or private credentials in a public repository.