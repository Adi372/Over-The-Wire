# Bandit Level 8 → Level 9

## Level Goal
Find the password in `data.txt`. It is the only line that occurs exactly once.

## Commands Used
```bash
sort data.txt | uniq -u
```

## Steps
1. Used `sort data.txt` to sort the lines so identical lines appear next to each other.
2. Piped the sorted output (`|`) into `uniq -u`.
3. `uniq -u` displayed only lines that occur exactly once.
4. The output revealed the password for the next level.

## Key Learning
- `sort`: Sorts lines of text.
- `uniq -u`: Displays lines that occur only once.
- `|` (pipe): Passes one command's output to another command.
- **Why sort first?** `uniq` checks adjacent lines, so sorting groups duplicate lines together.

## Next Level Login
```bash
ssh -p 2220 bandit9@bandit.labs.overthewire.org
```

**Note:** Save the password privately. Do not include it in the README.
