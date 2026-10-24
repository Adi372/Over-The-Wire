# Bandit Level 19 → Level 20

## Challenge Information

- **Platform:** OverTheWire Bandit
- **Category:** Linux, File Permissions, Privilege Escalation
- **Difficulty:** Beginner
- **Tools Used:** `ls`, `file`, `whoami`, `cat`
- **Objective:** Use the provided setuid executable to run a command with `bandit20` privileges and retrieve the password for the next level.

## 1. Challenge Description

The home directory contains an executable named `bandit20-do`. The challenge requires using this program to execute commands with the effective user ID of its owner.

This provides a way to read the password file for `bandit20` without logging in as that user first.

## 2. Initial Enumeration

I listed the files in the home directory:

```bash
ls
```

The output showed the `bandit20-do` executable.

I then checked its file type:

```bash
file bandit20-do
```

The output identified it as an ELF executable.

Next, I inspected its permissions and ownership:

```bash
ls -l
```

The permissions included:

```text
-rwsr-x---
```

The `s` in the owner's permission section indicates that the setuid bit is enabled alongside the owner's execute permission.

## 3. Understanding the Setuid Binary

**Setuid** stands for *set user ID*. When an executable has the setuid bit enabled, it can run with the effective user ID of the file's owner, subject to system rules.

I executed the binary without arguments to understand how to use it:

```bash
./bandit20-do
```

The program displayed usage instructions indicating that it accepts a command to execute.

The `./` prefix tells Linux to run the executable located in the current directory.

## 4. Verifying the Effective User

To check which user identity the program uses when executing a command, I ran:

```bash
./bandit20-do whoami
```

The output was:

```text
bandit20
```

This confirmed that the command was executed with `bandit20` as its effective user identity.

This matters because access to protected files is checked against the process's effective credentials, not simply the username shown in the original login session.

## 5. Troubleshooting Incorrect Commands

I also tried the following commands:

```bash
./bandit20-do ls
./bandit20-do cat /etc/bandit_pass/
./bandit20-do cd /etc/bandit_pass/
```

These attempts helped clarify how the executable works.

- `./bandit20-do ls` runs `ls`, which lists the current directory.
- `./bandit20-do cat /etc/bandit_pass/` attempts to read a directory as though it were a regular file. `cat` cannot display a directory's contents this way.
- `./bandit20-do cd /etc/bandit_pass/` does not work because `cd` is a shell built-in, not a standalone executable that the program can run directly.

To list the password directory, a command such as `./bandit20-do ls /etc/bandit_pass/` would be more appropriate. However, listing the directory is unnecessary when the exact password file path is already known.

## 6. Retrieving the Next Password

I ran the following command:

```bash
./bandit20-do cat /etc/bandit_pass/bandit20
```

The executable launched `cat` with the effective privileges provided by the setuid program. This allowed it to read the password file for `bandit20`.

The password was displayed in the terminal. I have intentionally excluded it from this write-up.

## 7. Key Learnings

- **`file`:** Identifies a file's type.
- **`ls -l`:** Displays file permissions, ownership, and other details.
- **Setuid:** Allows an executable to run with the effective user ID of its owner, subject to system rules.
- **`./bandit20-do command`:** Uses the provided executable to run a command with its configured privileges.
- **`whoami`:** Displays the effective username for the command's execution context.
- **`cat`:** Reads and displays file contents.
- **`cd`:** A shell built-in that changes the current shell's working directory; it cannot normally be invoked as an independent executable.
- **Effective user ID (EUID):** The identity the operating system generally uses when checking a process's permissions.

## 8. Next Level Login

After retrieving the password, the next step is to connect as `bandit20`:

```bash
ssh -p 2220 bandit20@bandit.labs.overthewire.org
```

Enter the password obtained from `/etc/bandit_pass/bandit20` when prompted.

## 9. References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [`file` manual](https://man7.org/linux/man-pages/man1/file.1.html)
- [`ls` manual](https://man7.org/linux/man-pages/man1/ls.1.html)
- [`chmod` and file permissions](https://man7.org/linux/man-pages/man1/chmod.1.html)
- [`cat` manual](https://man7.org/linux/man-pages/man1/cat.1.html)

**Security note:** Setuid executables can introduce serious security risks when poorly designed. Only inspect or execute them on systems where you have authorization. Never commit passwords or private credentials to a public repository.