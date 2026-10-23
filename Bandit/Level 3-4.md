# Bandit Level 3 → Level 4

## Level Goal
Find the password for Level 4 stored in a hidden file inside the `inhere` directory.

## Commands Used
```bash
ls
cd inhere/
ls
ls -la
cat ./...Hiding-From-You
```

## Steps
1. Run `ls` to locate the `inhere` directory.
2. Use `cd inhere/` to enter the directory.
3. Run `ls -la` to display all files, including hidden files.
4. Identify the hidden file named `...Hiding-From-You`.
5. Use `cat ./...Hiding-From-You` to read its contents and retrieve the password.

## Key Learning
- **`cd`** — Changes the current directory.
- **`ls -la`** — Lists all files, including hidden files, with detailed information.
- **Hidden files** — In Linux, filenames beginning with `.` are hidden by default.
- **`cat`** — Displays the contents of a file.

## Next Level Login
```bash
ssh -p 2220 bandit4@bandit.labs.overthewire.org
```

**Note:** `ls` alone did not show the hidden file. Using `ls -la` revealed it.