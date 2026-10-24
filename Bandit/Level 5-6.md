# Bandit Level 5 → Level 6

## Challenge Information

- **Platform:** OverTheWire Bandit
- **Category:** Linux / Command Line
- **Difficulty:** Beginner
- **Tools Used:** SSH, Linux Terminal, `find`
- **Objective:** Find the password for Level 6 in a file under the `inhere` directory that is human-readable, exactly 1033 bytes in size, and not executable.

## 1. Challenge Description

The password for the next level is stored in a file inside the `inhere` directory. The correct file must meet three conditions: it must be exactly 1033 bytes in size, must be human-readable, and must not be executable.

## 2. Initial Enumeration

Navigate into the `inhere` directory.

```bash
cd inhere/
```

**Observations:**
- The directory contains multiple subdirectories and files.
- Manually checking every file would be inefficient.

## 3. Analysis and Investigation

Use `find` to search for regular files that are exactly 1033 bytes in size and executable.

```bash
find . -size 1033c -type f -executable
```

**Mistake:** The `-executable` condition searches for executable files, which do not match the challenge requirements.

Next, exclude executable files:

```bash
find . -size 1033c -type f ! -executable
```

**Observations:**
- The matching file is `./maybehere07/.file2`.
- It satisfies the specified size and non-executable conditions.

**Key Finding:** Combining multiple `find` conditions narrows the search to the file that matches the challenge's requirements.

## 4. Solution

1. Enter the `inhere` directory.
2. Search for regular files exactly 1033 bytes in size.
3. Exclude executable files using `! -executable`.
4. Locate `./maybehere07/.file2`.
5. Display the file contents to retrieve the password.

```bash
cat ./maybehere07/.file2
```

Log in to Level 6 using the retrieved password:

```bash
ssh -p 2220 bandit6@bandit.labs.overthewire.org
```

## 5. Flag / Password

- **Password:** Retrieved from `./maybehere07/.file2`.

*Note: Avoid publishing the actual password in a public repository.*

## 6. What I Learned

- **`find`** — Searches for files and directories based on specified conditions.
- **`-size 1033c`** — Matches files whose size is exactly 1033 bytes; `c` specifies bytes.
- **`-type f`** — Restricts results to regular files.
- **`! -executable`** — Excludes files that are executable by the invoking user.
- **`cat`** — Displays the contents of a file.
- **Combining conditions** — Multiple search criteria help locate specific files efficiently.

## 7. Mistakes and Corrections

- **Incomplete `-exec` option:** The `find` command's `-exec` action requires a command and a terminating `\;` or `+`. It was unnecessary for this challenge.
- **Used `-executable` instead of `! -executable`:** The first searches for executable files; the second excludes them.

## 8. References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [Linux `find` command manual](https://man7.org/linux/man-pages/man1/find.1.html)