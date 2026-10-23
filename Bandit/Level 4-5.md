# Bandit Level 4 → Level 5

## Level Goal
Find the password for Level 5 stored in the only human-readable file inside the `inhere` directory.

## Commands Used
```bash
ls
cd inhere/
ls
file ./*
cat ./-file07
```

## Steps
1. Run `ls` to locate the `inhere` directory.
2. Use `cd inhere/` to enter the directory.
3. Run `ls` to list the ten files.
4. Use `file ./*` to identify each file's type.
5. Find the file identified as `ASCII text`.
6. Use `cat ./-file07` to display its contents and retrieve the password.

## Key Learning
- **`file`** — Identifies the type of a file.
- **`./*`** — Matches all non-hidden entries in the current directory.
- **`ASCII text`** — Indicates readable text using the ASCII character set.
- **`cat`** — Displays the contents of a file.

## Next Level Login
```bash
ssh -p 2220 bandit5@bandit.labs.overthewire.org
```

**Note:** The other files were identified as `data`, so `file ./*` helped locate the only human-readable file without opening each file individually.