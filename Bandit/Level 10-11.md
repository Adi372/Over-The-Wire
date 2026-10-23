# Bandit Level 10 → Level 11

## Level Goal
Decode the Base64-encoded data in `data.txt` to find the next level's password.

## Commands Used
```bash
cat data.txt | base64 -d
```

## Steps
1. Used `cat data.txt` to display the encoded content.
2. Piped the output (`|`) into `base64 -d` to decode it.
3. The decoded output revealed the password for the next level.

## Key Learning
- **Base64:** An encoding method that represents data using printable characters. It is not encryption.
- `base64 -d`: Decodes Base64-encoded data.
- `|` (pipe): Passes one command's output as input to another command.

## Next Level Login
```bash
ssh -p 2220 bandit11@bandit.labs.overthewire.org
```

**Note:** Save the password privately. Do not include it in the README.
