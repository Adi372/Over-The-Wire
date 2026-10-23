# Bandit Level 5 → Level 6

## Level Goal
Find the password for Level 6 in a file under the `inhere` directory that is human-readable, exactly 1033 bytes in size, and not executable.

## Commands Used
```bash
cd inhere/
find . -size 1033c -type f -executable
find . -size 1033c -type f ! -executable
cat ./maybehere07/.file2
```

## Steps
1. Enter the `inhere` directory.
2. Use `find` to search for files that are exactly 1033 bytes in size.
3. Exclude executable files using `! -executable`.
4. Locate the matching file at `./maybehere07/.file2`.
5. Use `cat` to display its contents and retrieve the password.

## Mistakes Made
- **Incomplete `-exec` option:** Running `find . -size 1033c -type f -exec` returned an error because `-exec` requires a command and a terminating `\;` or `+`.
- **Used `-executable` first:** This searches for executable files, which did not match the level requirements. Using `! -executable` correctly excludes them.

## Key Learning
- **`find`** — Searches for files and directories based on specific conditions.
- **`-size 1033c`** — Matches files exactly 1033 bytes in size (`c` means bytes).
- **`-type f`** — Searches only for regular files.
- **`! -executable`** — Excludes executable files.
- **`cat`** — Displays file contents.

## Next Level Login
```bash
ssh -p 2220 bandit6@bandit.labs.overthewire.org
```

**Note:** The `find` command can combine multiple conditions to narrow down a search efficiently.