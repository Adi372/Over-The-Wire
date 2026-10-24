# Bandit Level 7 → Level 8

## Challenge Information

- **Platform:** OverTheWire Bandit
- **Category:** Linux / Command Line
- **Difficulty:** Beginner
- **Tools Used:** SSH, Linux Terminal, `grep`
- **Objective:** Find the password for Level 8 in `data.txt`, next to the word `millionth`.

## 1. Challenge Description

The password for the next level is stored in `data.txt`, alongside the word `millionth`. The goal is to search the file for the specified word and retrieve the password from the matching line.

## 2. Initial Enumeration

List the files in the home directory.

```bash
ls
```

**Observations:**
- The file `data.txt` is present in the home directory.

## 3. Analysis and Investigation

Use `cat` to read the file and pipe its output to `grep` to find the matching line.

```bash
cat data.txt | grep "millionth"
```

**Observations:**
- `grep` filters the input and displays the line containing `millionth`.
- The password appears next to the matching word.

**Key Finding:** Searching for a unique word is more efficient than manually inspecting a large file.

## 4. Solution

1. Run `ls` to confirm that `data.txt` exists.
2. Execute `cat data.txt` to display the file contents.
3. Use the pipe operator (`|`) to pass the output to `grep`.
4. Search for `millionth` using `grep "millionth"`.
5. Copy the password from the matching line.
6. Log in to Level 8 using SSH.

```bash
ssh -p 2220 bandit8@bandit.labs.overthewire.org
```

Enter the retrieved password when prompted.

**Simpler alternative:**

```bash
grep "millionth" data.txt
```

This works directly because `grep` can search a file without needing `cat` or a pipe.

## 5. Flag / Password

- **Password:** Retrieved from the line containing `millionth` in `data.txt`.

*Note: Keep the password private and avoid publishing it in your README.*

## 6. What I Learned

- **`cat`** — Displays the contents of a file.
- **`grep`** — Searches text for a matching pattern.
- **Pipe (`|`)** — Sends the standard output of one command to the standard input of another.
- **Text searching** — Searching for a known word is more efficient than reading a large file manually.

## 7. References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [GNU Grep Manual](https://www.gnu.org/software/grep/manual/grep.html)