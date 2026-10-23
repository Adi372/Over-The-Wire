# Bandit Level 18 → 19

## Level Goal

The password for the next level is stored in a file named `readme` in the home directory. However, the `.bashrc` file has been modified to log users out immediately after SSH login.

## Commands Used

```bash
ssh -p 2220 bandit18@bandit.labs.overthewire.org
ssh -p 2220 bandit18@bandit.labs.overthewire.org cat readme
ssh -p 2220 bandit19@bandit.labs.overthewire.org
```

## Steps

1. Tried logging in to Bandit Level 18 normally using SSH.
2. The connection displayed `Byebye!` and closed immediately.
3. Used SSH to execute `cat readme` directly on the remote server without opening an interactive shell.
4. Retrieved the password for Bandit Level 19 from the `readme` file.
5. Connected to Bandit Level 19 using the retrieved password.

## Key Learning

- **`ssh`** securely connects to a remote machine.
- **`-p 2220`** specifies the SSH port.
- **`cat readme`** displays the contents of the `readme` file.
- **Remote command execution:** You can provide a command after the SSH destination to execute it remotely without starting an interactive shell.
- **`.bashrc`** is a shell configuration file that can run commands when Bash starts. A modified configuration can interfere with normal login sessions.

## Next Level Login

- Username: `bandit19`
- SSH host: `bandit.labs.overthewire.org`
- Port: `2220`

## Note

The key to this level was executing a command directly through SSH instead of opening a normal interactive shell. Avoid publishing passwords in public repositories.