# Bandit Level 0 → Level 1

## Level Goal
Find the password for Level 1 stored in a file named `readme` in the home directory.

## Commands Used
```bash
ls
cat readme
```

## Steps
1. Run `ls` to list the files in the home directory.
2. Locate the `readme` file.
3. Run `cat readme` to display its contents.
4. Copy the password shown in the terminal.
5. Use the password to log in as `bandit1` via SSH on port `2220`.

## Key Learning
- **`ls`** — Lists files and directories.
- **`cat`** — Displays the contents of a file.
- **SSH** — Connects to a remote server.

## Next Level Login
```bash
ssh -p 2220 bandit1@bandit.labs.overthewire.org
```

**Note:** Save the password securely in your local notes for future use.