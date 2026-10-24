# Bandit Level 4 → Level 5

## Challenge Information

- **Platform:** OverTheWire Bandit
- **Category:** Linux / Command Line
- **Difficulty:** Beginner
- **Tools Used:** SSH, Linux Terminal
- **Objective:** Find the password for Level 5 stored in the only human-readable file inside the `inhere` directory.

## 1. Challenge Description

The password for the next level is stored in one of the files inside the `inhere` directory. Most files contain non-text data, and only one contains human-readable text. The goal is to identify that file and retrieve the password.

## 2. Initial Enumeration

List the files and directories in the home directory.

```bash
ls
```

**Observations:**
- A directory named `inhere` is present.

Navigate into the directory:

```bash
cd inhere/
```

List the files:

```bash
ls
```

**Observations:**
- The directory contains ten files with similar names.

## 3. Analysis and Investigation

Instead of opening each file individually, use the `file` command to identify their types.

```bash
file ./*
```

**Observations:**
- Most files are identified as `data`, indicating that they contain data not recognized as a standard text file.
- The file `-file07` is identified as `ASCII text`.

**Key Finding:** The file `-file07` is the only human-readable text file among the files examined.

## 4. Solution

1. Run `ls` to locate the `inhere` directory.
2. Execute `cd inhere/` to enter the directory.
3. Use `ls` to list the ten files.
4. Run `file ./*` to identify the type of each file.
5. Locate the file identified as `ASCII text`.
6. Execute `cat ./-file07` to display its contents.
7. Copy the password and log in to the next level.

```bash
cat ./-file07
```

Log in to Level 5:

```bash
ssh -p 2220 bandit5@bandit.labs.overthewire.org
```

Enter the retrieved password when prompted.

## 5. Flag / Password

- **Password:** Retrieved from `-file07`.

*Note: Avoid publishing the actual password in a public repository.*

## 6. What I Learned

- **`file`** — Identifies a file's type by examining its contents and other characteristics.
- **`./*`** — Expands to non-hidden entries in the current directory.
- **ASCII text** — Text represented using the ASCII character encoding.
- **`cat`** — Displays the contents of a file.
- **Efficient enumeration** — Identifying file types can be faster than opening files individually.

## 7. References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [Linux `file` command manual](https://man7.org/linux/man-pages/man1/file.1.html)