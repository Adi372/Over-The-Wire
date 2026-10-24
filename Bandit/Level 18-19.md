# Bandit Level 18 → Level 19

## Challenge Information

- **Platform:** OverTheWire Bandit
- **Category:** Linux, SSH, Remote Command Execution
- **Difficulty:** Beginner
- **Tools Used:** `ssh`, `cat`
- **Objective:** Retrieve the password for `bandit19` from the `readme` file despite the modified shell configuration that immediately terminates normal SSH sessions.

## 1. Challenge Description

The password for the next level is stored in a file named `readme` in the home directory.

However, the `.bashrc` file has been modified so that a normal SSH login displays a message and immediately closes the session. This prevents the user from accessing a regular interactive shell.

The solution is to execute a command directly through SSH without opening an interactive shell.

## 2. Attempting a Normal SSH Login

I first tried connecting to `bandit18` normally:

```bash
ssh -p 2220 bandit18@bandit.labs.overthewire.org
```

The connection displayed:

```text
Byebye!
```

The session then terminated.

This indicated that the account's shell configuration was interfering with normal interactive access.

## 3. Executing a Remote Command

Instead of opening an interactive shell, I supplied the command directly after the SSH destination:

```bash
ssh -p 2220 bandit18@bandit.labs.overthewire.org cat readme
```

### Command breakdown

- `ssh` establishes a secure connection to the remote server.
- `-p 2220` specifies the SSH port.
- `bandit18@bandit.labs.overthewire.org` identifies the remote account and host.
- `cat readme` is the command executed on the remote machine.

SSH sends the command to the remote system for execution. The output of `cat readme` is returned to my terminal without requiring a normal interactive shell.

The command displayed the password for `bandit19`. I kept the actual password out of this write-up.

## 4. Logging In to bandit19

After retrieving the password, I connected to the next account:

```bash
ssh -p 2220 bandit19@bandit.labs.overthewire.org
```

I entered the password obtained from `readme` when prompted.

## 5. Key Learnings

- **`ssh`:** Establishes a secure connection to a remote machine.
- **`-p 2220`:** Specifies the port used by the SSH server.
- **`cat readme`:** Displays the contents of the `readme` file.
- **Remote command execution:** SSH can execute a specified command remotely without starting a normal interactive shell.
- **`.bashrc`:** A Bash configuration file that can execute commands when an interactive non-login Bash shell starts.
- **Interactive vs. non-interactive execution:** A remote command can sometimes succeed even when an interactive login session is restricted.

**Important distinction:** `.bashrc` normally configures interactive Bash shells. In this challenge, the modified configuration causes the immediate logout behavior; it is not a general rule that `.bashrc` always runs for every SSH command.

## 6. References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [`ssh` manual](https://man7.org/linux/man-pages/man1/ssh.1.html)
- [`cat` manual](https://man7.org/linux/man-pages/man1/cat.1.html)
- [GNU Bash Startup Files](https://www.gnu.org/software/bash/manual/html_node/Bash-Startup-Files.html)

**Security note:** Never publish actual passwords or private SSH keys in a public repository.