# Bandit Level 11 → Level 12

## Level Goal
Decode the contents of `data.txt`. All uppercase and lowercase letters have been rotated by 13 positions using ROT13.

## Commands Used
```bash
cat data.txt | tr "N-ZA-Mn-za-m" "A-Za-z"
```

## Steps
1. Used `cat data.txt` to display the encoded text.
2. Piped the output (`|`) into `tr` to translate the characters.
3. The `tr` command mapped the rotated letters back to their original letters using ROT13.
4. The decoded output revealed the next level's password.

## Key Learning
- **ROT13:** A letter substitution method that rotates each letter by 13 positions.
- `tr`: Translates or replaces characters.
- `N-ZA-Mn-za-m`: Defines the rotated uppercase and lowercase letter ranges.
- `A-Za-z`: Defines the normal uppercase and lowercase alphabet ranges.
- `|` (pipe): Passes one command's output to another command.

## Next Level Login
```bash
ssh -p 2220 bandit12@bandit.labs.overthewire.org
```

**Note:** Save the password privately. Do not include it in the README.
