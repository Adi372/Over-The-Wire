# Bandit Level 7 → Level 8

## Level Goal
Find the password for the next level in `data.txt`, next to the word `millionth`.

## Commands Used
```bash
ls
cat data.txt | grep "millionth"
```

## Steps
1. Used `ls` to confirm that `data.txt` exists.
2. Used `cat data.txt` to display the file contents and piped the output (`|`) to `grep`.
3. `grep "millionth"` filtered the output to show the line containing the word `millionth`.
4. The password appeared next to the matching word.

## Key Learning
- `cat`: Displays file contents.
- `grep`: Searches text for a matching pattern.
- `|` (pipe): Sends the output of one command as input to another command.

## Next Level Login
```bash
ssh -p 2220 bandit8@bandit.labs.overthewire.org
```

**Note:** Save the password privately for the next level. Do not publish it in your README.
