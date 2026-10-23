# Bandit Level 19 → 20

## Level Goal

Use the setuid binary in the home directory to execute commands with the permissions of another user. After using the binary, retrieve the next password from `/etc/bandit_pass`.

## Commands Used

```bash
ls
file bandit20-do
ls -l
./bandit20-do
./bandit20-do whoami
./bandit20-do ls
./bandit20-do cat /etc/bandit_pass/
./bandit20-do cd /etc/bandit_pass/
./bandit20-do cat /etc/bandit_pass/bandit20
```

## Steps

1. Used `ls` to find the `bandit20-do` executable in the home directory.
2. Used `file bandit20-do` to identify the file as a setuid ELF executable.
3. Used `ls -l` to inspect its permissions. The `s` in `-rwsr-x---` indicates the setuid bit is enabled.
4. Executed `./bandit20-do` without arguments to learn how to use it.
5. Ran `./bandit20-do whoami`, which returned `bandit20`. This confirmed that the program executes commands as the `bandit20` user.
6. Tried to list `/etc/bandit_pass/` and change directories using the binary. The directory listing attempt failed because the path itself was a directory, and `cd` failed because it is a shell built-in rather than a standalone executable.
7. Ran `./bandit20-do cat /etc/bandit_pass/bandit20` to read the password for the next level.

## Key Learning

- **`file`** identifies a file's type.
- **`ls -l`** displays file permissions and ownership.
- **setuid** allows an executable to run with the effective user ID of its owner, subject to system rules.
- **`./bandit20-do command`** runs a command using the permissions provided by the setuid program.
- **`whoami`** shows the effective user identity for this execution.
- **`cat`** reads file contents.
- **`cd`** is a shell built-in, so it cannot be executed like a normal external program through this binary.

## Next Level Login

- Username: `bandit20`
- SSH host: `bandit.labs.overthewire.org`
- Port: `2220`

## Note

The important concept in this level is **setuid privilege**: the executable lets you run commands as its owner, `bandit20`. Never run unfamiliar setuid binaries on a system you do not own or have permission to test.

Do not include passwords in public repositories.