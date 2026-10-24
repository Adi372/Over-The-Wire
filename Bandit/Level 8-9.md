# Bandit Level 8 → Level 9

## Challenge Information

- **Platform:** OverTheWire Bandit
- **Category:** Linux / Command Line
- **Difficulty:** Beginner
- **Tools Used:** SSH, Linux Terminal, `sort`, `uniq`
- **Objective:** Find the password in `data.txt`. It is the only line that occurs exactly once.

## 1. Challenge Description

The password for the next level is stored in `data.txt`, which contains multiple lines of text. The goal is to identify the line that appears exactly once and retrieve the password.

## 2. Initial Enumeration

List the files in the home directory.

```bash
ls
```

**Observations:**
- The file `data.txt` is present in the home directory.
- The file contains multiple lines, including repeated entries.

## 3. Analysis and Investigation

Sort the file's contents and pass the result to `uniq -u`.

```bash
sort data.txt | uniq -u
```

**Observations:**
- `sort data.txt` arranges the lines so identical entries appear consecutively.
- `uniq -u` displays only lines that occur exactly once among adjacent lines.
- The output reveals the password for the next level.

**Key Finding:** Sorting the file first allows `uniq` to identify repeated and unique lines correctly.

## 4. Solution

1. Run `ls` to confirm that `data.txt` exists.
2. Execute `sort data.txt` to group identical lines together.
3. Pipe the sorted output into `uniq -u`.
4. Identify the single line displayed as the password.
5. Copy the password and log in to Level 9.

```bash
sort data.txt | uniq -u
```

Log in using SSH:

```bash
ssh -p 2220 bandit9@bandit.labs.overthewire.org
```

Enter the retrieved password when prompted.

## 5. Flag / Password

- **Password:** Retrieved using `sort data.txt | uniq -u`.

*Note: Keep the password private and avoid publishing it in your README.*

## 6. What I Learned

- **`sort`** — Sorts lines of text, grouping identical lines together.
- **`uniq -u`** — Displays lines that occur exactly once among adjacent lines.
- **Pipe (`|`)** — Passes one command's output to another command as input.
- **Why sort first?** — `uniq` compares adjacent lines rather than searching the entire file for every occurrence. Sorting ensures identical lines are grouped together.

## 7. References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [GNU Coreutils — `sort`](https://www.gnu.org/software/coreutils/manual/html_node/sort-invocation.html)
- [GNU Coreutils — `uniq`](https://www.gnu.org/software/coreutils/manual/html_node/uniq-invocation.html)