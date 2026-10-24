# Bandit Level 11 → Level 12

## Challenge Information

- **Platform:** OverTheWire Bandit
- **Category:** Linux / Cryptography and Encoding
- **Difficulty:** Beginner
- **Tools Used:** SSH, Linux Terminal, `tr`
- **Objective:** Decode the contents of `data.txt`, where uppercase and lowercase letters have been rotated by 13 positions using ROT13.

## 1. Challenge Description

The password for the next level is stored in `data.txt`. Each uppercase and lowercase letter has been shifted by 13 positions using ROT13. The goal is to reverse this transformation and retrieve the original text.

## 2. Initial Enumeration

List the files in the home directory.

```bash
ls
```

**Observations:**
- The file `data.txt` is present in the home directory.
- Its contents are encoded using ROT13.

## 3. Analysis and Investigation

Use the `tr` command to translate the rotated letters back to their original characters.

```bash
cat data.txt | tr "N-ZA-Mn-za-m" "A-Za-z"
```

**Observations:**
- `cat data.txt` reads the encoded text.
- The pipe (`|`) passes the output to `tr`.
- `N-ZA-M` maps the rotated uppercase letters back to `A-Z`.
- `n-za-m` maps the rotated lowercase letters back to `a-z`.
- The decoded output reveals the password for the next level.

**Key Finding:** ROT13 is reversible by applying the same rotation again, because rotating a letter by 13 positions twice returns it to its original position.

## 4. Solution

1. Run `ls` to confirm that `data.txt` exists.
2. Execute `cat data.txt` to read the encoded content.
3. Pipe the output into `tr` with the appropriate character mappings.
4. Copy the decoded password.
5. Log in to Level 12 using SSH.

```bash
cat data.txt | tr "N-ZA-Mn-za-m" "A-Za-z"
```

Log in using SSH:

```bash
ssh -p 2220 bandit12@bandit.labs.overthewire.org
```

Enter the retrieved password when prompted.

**Simpler alternative:**

```bash
tr 'A-Za-z' 'N-ZA-Mn-za-m' < data.txt
```

This performs the ROT13 transformation directly from the file without using `cat` or a pipe.

## 5. Flag / Password

- **Password:** Retrieved from the decoded contents of `data.txt`.

*Note: Keep the password private and avoid publishing it in your README.*

## 6. What I Learned

- **ROT13** — A letter-substitution technique that rotates each letter by 13 positions.
- **`tr`** — Translates or replaces characters in input.
- **Character ranges** — `A-Z` and `a-z` represent uppercase and lowercase letters.
- **Pipe (`|`)** — Passes one command's output to another command as input.
- **Encoding vs. encryption** — ROT13 is a simple substitution encoding and does not provide meaningful security.

## 7. References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [GNU Coreutils — `tr`](https://www.gnu.org/software/coreutils/manual/html_node/tr-invocation.html)