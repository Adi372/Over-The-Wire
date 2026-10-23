# Bandit Level 17 → 18

## Level Goal

There are two files in the home directory: `passwords.old` and `passwords.new`. The password for the next level is the only line that changed between these files.

## Commands Used

```bash
diff passwords.new passwords.old
exit
ssh -p 2220 bandit18@bandit.labs.overthewire.org
```

## Steps

1. Connected to Bandit Level 17 using the SSH private key saved from the previous level.
2. Ran `diff passwords.new passwords.old` to compare the two files.
3. Identified the changed line. The line marked with `<` came from `passwords.new`, while the line marked with `>` came from `passwords.old`.
4. Used the changed line from `passwords.new` as the password for Bandit Level 18.
5. Exited the Level 17 session and attempted to log in to Bandit Level 18 using SSH.
6. The connection displayed `Byebye!` and closed automatically.

## Key Learning

- **`diff`** compares two files and displays their differences.
- **`<`** indicates a line from the first file specified in the command.
- **`>`** indicates a line from the second file specified in the command.
- The order of filenames matters when interpreting `diff` output.
- A successful password discovery does not guarantee that the next SSH session will remain open.

## Note

The `Byebye!` message is expected for this level transition. Bandit Level 18 restricts normal SSH login sessions, so the next challenge involves understanding how the remote shell behaves.

Do not publish passwords or private keys in a public repository.