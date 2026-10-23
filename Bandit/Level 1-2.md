# Bandit Level 1 → Level 2

## Level Goal
Find the password for Level 2 stored in a file named `-` in the home directory.

## Commands Used
```bash
ls
cat -*
cat ./-
```

## Steps
1. Run `ls` to list the files in the home directory.
2. Identify the file named `-`.
3. Use `cat ./-` to display the file contents and retrieve the password.

## Mistakes Made
- **Used `cat -*`:** The `*` is a wildcard that matches filenames. However, the resulting filename `-` is interpreted as an option by `cat`, not as a regular filename.
- **Correct approach — `cat ./-`:** The `./` prefix tells the shell to access the file named `-` in the current directory, avoiding the option-parsing issue.

## Key Learning
- **`ls`** — Lists files and directories.
- **`cat`** — Displays file contents.
- **`./`** — Refers to the current directory and helps handle filenames beginning with a hyphen.

## Next Level Login
```bash
ssh -p 2220 bandit2@bandit.labs.overthewire.org
```

**Note:** Save the password in your local notes for future use.