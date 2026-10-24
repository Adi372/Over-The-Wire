# Bandit Level 13 → Level 14

## Challenge Information

- **Platform:** OverTheWire Bandit
- **Category:** Linux, SSH Authentication, File Permissions
- **Difficulty:** Beginner
- **Tools Used:** `ls`, `cat`, `scp`, `chmod`, `ssh`
- **Objective:** Use the provided private SSH key to log in as `bandit14` and retrieve the password for the next level.

## 1. Challenge Description

The private SSH key required to authenticate as `bandit14` is provided in the `bandit13` home directory. Instead of using a password to log in, the key must be used for SSH authentication.

After logging in as `bandit14`, the password for the next level can be read from:

`/etc/bandit_pass/bandit14`

## 2. Initial Enumeration

First, I listed the files in the current home directory.

```bash
ls
```

The output showed two files:

- `sshkey.private` — the private SSH key.
- `HINT` — additional instructions for the challenge.

I read the hint file:

```bash
cat HINT
```

This confirmed that the provided SSH key could be used to access the next account.

## 3. Copying the Private Key

I copied the private key from the remote Bandit server to my local machine using `scp`.

```bash
scp -P 2220 bandit13@bandit.labs.overthewire.org:sshkey.private .
```

### Command breakdown

- `scp` securely copies files between computers over SSH.
- `-P 2220` specifies the SSH port used by the Bandit server.
- `bandit13@bandit.labs.overthewire.org:sshkey.private` specifies the remote account, server, and file.
- `.` specifies the current local directory as the destination.

The key was saved in my local working directory. If needed, I could navigate to my `Documents` directory before running the command.

## 4. Setting Secure File Permissions

Before using the private key, I restricted its permissions:

```bash
chmod 600 sshkey.private
```

This gives the file owner read and write permissions while removing all permissions for the group and other users.

SSH private keys should be protected because anyone who obtains an exposed, usable key may be able to authenticate as its associated account.

## 5. Logging In with the Private Key

I used the `-i` option to specify the private key during SSH authentication.

```bash
ssh -i sshkey.private -p 2220 bandit14@bandit.labs.overthewire.org
```

### Command breakdown

- `ssh` starts a secure remote connection.
- `-i sshkey.private` specifies the identity file used for authentication.
- `-p 2220` specifies the remote SSH port.
- `bandit14@bandit.labs.overthewire.org` identifies the target account and server.

The connection succeeded, allowing me to access the server as `bandit14` without entering that account's password.

## 6. Retrieving the Next Password

Once logged in as `bandit14`, I read the password file:

```bash
cat /etc/bandit_pass/bandit14
```

The command displayed the password required for the next level.

The path is an **absolute path** because it starts with `/`, the root directory of the Linux filesystem.

**Password handling:** The password is intentionally not included in this write-up. Store it securely and avoid committing it to a public repository.

## 7. Key Learnings

- **`scp`:** Transfers files securely between local and remote systems over SSH.
- **`chmod 600`:** Restricts file access to the owner, with read and write permissions.
- **`ssh -i`:** Specifies a private key for SSH authentication.
- **Private SSH key:** A credential used to prove identity during public-key authentication.
- **Absolute path:** A path that starts from the filesystem root, such as `/etc/bandit_pass/bandit14`.
- **Authentication:** SSH can authenticate users using a private key instead of an account password.

## 8. Next Level

Successfully logging in as `bandit14` completes this level. The password retrieved from `/etc/bandit_pass/bandit14` is used to authenticate to the next level using the method specified by its challenge.

## 9. References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [`scp` manual](https://man7.org/linux/man-pages/man1/scp.1.html)
- [`chmod` manual](https://man7.org/linux/man-pages/man1/chmod.1.html)
- [`ssh` manual](https://man7.org/linux/man-pages/man1/ssh.1.html)

**Security note:** Never publish private SSH keys or passwords in a public repository. Keep credentials private and use restrictive file permissions.