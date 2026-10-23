# Bandit Level 13 → Level 14

## Level Goal
Use the provided private SSH key to log in as `bandit14`. The password is stored in `/etc/bandit_pass/bandit14`, which only `bandit14` can read.

## Commands Used
```bash
ls
cat HINT
scp -P 2220 bandit13@bandit.labs.overthewire.org:sshkey.private .
chmod 600 sshkey.private
ssh -i sshkey.private -p 2220 bandit14@bandit.labs.overthewire.org
cat /etc/bandit_pass/bandit14
```

## Steps
1. Used `ls` to find `sshkey.private` and `HINT` in the home directory.
2. Read `HINT` to check the instructions for this level.
3. Used `scp` to copy the private key from the Bandit server to the local `Documents` directory.
4. Used `chmod 600` to restrict the key's permissions so only the owner can read and write it.
5. Used `ssh -i` to log in as `bandit14` with the private key instead of a password.
6. Read `/etc/bandit_pass/bandit14` to retrieve the password.

## Key Learning
- `scp`: Securely copies files between computers over SSH.
- `chmod 600`: Gives the owner read/write permissions and removes permissions for group and others.
- `ssh -i`: Uses a specified private key for authentication.
- **Private SSH key:** A credential used to prove your identity without entering an account password.
- **Absolute path:** `/etc/bandit_pass/bandit14` starts from the filesystem root.

## Next Level Login
You have successfully accessed `bandit14`. The password for the next level is obtained from the file above.

**Note:** Never publish private keys or passwords in a public repository. Keep them stored securely.
