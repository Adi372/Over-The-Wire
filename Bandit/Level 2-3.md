# Bandit Level 2 → Level 3

## Level Goal
Find the password for Level 3 stored in a file named `--spaces in this filename--` in the home directory.

## Commands Used
```bash
ls
cat "--spaces in this filename--"
cat ./--spaces\ in\ this\ filename--
```

## Steps
1. Run `ls` to list the files in the home directory.
2. Identify the file containing spaces in its name.
3. Try to read the file using `cat` with quotes.
4. Since the filename starts with hyphens, use `./` and escape the spaces to read its contents.

## Mistakes Made
- **Used quotes without `./`:** `cat "--spaces in this filename--"` passes the filename as one argument, but the filename begins with `--`, so `cat` interprets it as an option.
- **Correct approach — `cat ./--spaces\ in\ this\ filename--`:** The `./` prefix makes it clear that the argument is a file path. The backslashes escape the spaces so the shell treats the filename as one argument.

## Key Learning
- **Spaces in filenames:** Use quotes or escape spaces with `\`.
- **Filenames starting with hyphens:** Prefix the filename with `./` to avoid option-parsing issues.
- **`cat`** — Displays the contents of a file.

## Next Level Login
```bash
ssh -p 2220 bandit3@bandit.labs.overthewire.org
```

**Note:** Save the password in your local notes for future use.