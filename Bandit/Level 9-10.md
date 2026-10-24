# Bandit Level 9 → Level 10

## Challenge Information

- **Platform:** OverTheWire Bandit
- **Category:** Linux / Command Line
- **Difficulty:** Beginner
- **Tools Used:** SSH, Linux Terminal, `strings`, `grep`
- **Objective:** Find the password in `data.txt`, hidden among human-readable strings and preceded by several `=` characters.

## 1. Challenge Description

The password for the next level is stored in `data.txt`, which contains non-text data. The goal is to extract the human-readable strings, search for lines containing `=`, and identify the password.

## 2. Initial Enumeration

List the files in the home directory.

```bash
ls
```

**Observations:**
- The file `data.txt` is present in the home directory.
- The file contains non-text data, making it difficult to read directly using `cat`.

## 3. Analysis and Investigation

Use `strings` to extract readable text from the file and pipe the output to `grep`.

```bash
strings data.txt | grep "="
```

**Observations:**
- `strings data.txt` extracts sequences of printable characters.
- `grep "="` filters the output to display lines containing the `=` character.
- One of the matching lines contains several `=` characters preceding the password.

**Key Finding:** Extracting readable strings makes it easier to find meaningful text within a file containing binary data.

## 4. Solution

1. Run `ls` to confirm that `data.txt` exists.
2. Execute `strings data.txt` to extract readable text.
3. Pipe the output into `grep "="`.
4. Examine the matching lines and identify the password preceded by several `=` characters.
5. Copy the password and log in to Level 10.

```bash
strings data.txt | grep "="
```

Log in using SSH:

```bash
ssh -p 2220 bandit10@bandit.labs.overthewire.org
```

Enter the retrieved password when prompted.

## 5. Flag / Password

- **Password:** Retrieved from the matching human-readable string in `data.txt`.

*Note: Keep the password private and avoid publishing it in your README.*

## 6. What I Learned

- **`strings`** — Extracts sequences of printable characters from binary or non-text files.
- **`grep "="`** — Filters lines containing the `=` character.
- **Pipe (`|`)** — Passes the output of one command to another command as input.
- **Binary file analysis** — Useful text can exist inside files that are not ordinary text documents.
- **Combining commands** — Piping `strings` into `grep` narrows the output and makes investigation more efficient.

## 7. References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [Linux `strings` command manual](https://man7.org/linux/man-pages/man1/strings.1.html)
- [GNU Grep Manual](https://www.gnu.org/software/grep/manual/grep.html)