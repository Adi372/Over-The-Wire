# Bandit Level 9 → Level 10

## Level Goal
Find the password in `data.txt`. It is hidden among human-readable strings and preceded by several `=` characters.

## Commands Used
```bash
strings data.txt | grep "="
```

## Steps
1. Used `strings data.txt` to extract readable text from the file, which contains non-text data.
2. Piped the output (`|`) into `grep "="` to show only lines containing the `=` character.
3. Identified the line with several `=` characters preceding the password.

## Key Learning
- `strings`: Extracts printable text from binary or non-text files.
- `grep "="`: Filters lines containing the `=` character.
- `|` (pipe): Sends one command's output to another command.

## Next Level Login
```bash
ssh -p 2220 bandit10@bandit.labs.overthewire.org
```

**Note:** Save the password privately. Do not include it in the README.
