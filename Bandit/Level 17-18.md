# Bandit Level 17 → Level 18

## Challenge Information

- **Platform:** OverTheWire Bandit
- **Category:** Linux, File Comparison, SSH
- **Difficulty:** Beginner
- **Tools Used:** `diff`, SSH
- **Objective:** Find the password for `bandit18` by identifying the only line that differs between `passwords.old` and `passwords.new`.

## 1. Challenge Description

The home directory contains two files:

- `passwords.old`
- `passwords.new`

The password for the next level is the only line that changed between these files. The task is to compare them and identify the correct password.

## 2. Comparing the Files

After connecting to `bandit17` using the private SSH key obtained in the previous level, I ran:

```bash
diff passwords.new passwords.old
```

The `diff` command compares two files line by line and displays their differences.

### Understanding the output

For this command:

- `<` indicates a line from the **first file**, `passwords.new`.
- `>` indicates a line from the **second file**, `passwords.old`.

The order of the filenames matters because `diff` uses it to determine which lines belong to each file.

I identified the changed line from `passwords.new` and used it as the password for `bandit18`. The actual password is omitted from this write-up.

## 3. Attempting to Log In to bandit18

I exited the current session:

```bash
exit
```

Then I attempted to connect to the next account:

```bash
ssh -p 2220 bandit18@bandit.labs.overthewire.org
```

After authentication, the server displayed:

```text
Byebye!
```

The connection then closed automatically.

## 4. Understanding the `Byebye!` Message

This behavior is expected in this challenge. The `bandit18` account is configured to close normal interactive SSH sessions, so successfully authenticating does not mean that a regular shell will remain available.

The next challenge requires understanding how SSH can execute a command remotely, rather than relying on a normal interactive shell.

## 5. Key Learnings

- **`diff`:** Compares files line by line and displays differences.
- **`<`:** Identifies a differing line from the first file supplied to `diff`.
- **`>`:** Identifies a differing line from the second file supplied to `diff`.
- **Filename order:** Determines how the differences are presented.
- **`exit`:** Ends the current shell session.
- **SSH authentication vs. shell access:** Authentication can succeed even when the server is configured to terminate an interactive session.

## 6. References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [`diff` manual](https://man7.org/linux/man-pages/man1/diff.1.html)
- [`ssh` manual](https://man7.org/linux/man-pages/man1/ssh.1.html)

**Security note:** Keep passwords and private SSH keys out of public repositories. Never include actual credentials in a public write-up.