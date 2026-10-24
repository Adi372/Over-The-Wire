# Bandit Level 0 → Level 1

## Challenge Information

- **Platform:** OverTheWire Bandit
- **Category:** Linux / Command Line
- **Difficulty:** Beginner
- **Tools Used:** SSH, Linux Terminal
- **Objective:** Find the password for Level 1 stored in a file named `readme` in the home directory.

## 1. Challenge Description

The password for the next level is stored in a file named `readme` in the home directory. The goal is to access the file, retrieve the password, and use it to log in as `bandit1`.

## 2. Initial Enumeration

First, list the files and directories in the home directory.

```bash
ls
```

**Observations:**
- A file named `readme` is present in the home directory.

## 3. Analysis and Investigation

Use the `cat` command to display the contents of the file.

```bash
cat readme
```

**Key Finding:** The file contains the password required to authenticate as `bandit1`.

## 4. Solution

1. Run `ls` to list the files in the home directory.
2. Identify the `readme` file.
3. Execute `cat readme` to read its contents.
4. Copy the displayed password.
5. Log in to the next level using SSH on port `2220`.

```bash
ssh -p 2220 bandit1@bandit.labs.overthewire.org
```

Enter the retrieved password when prompted.

## 5. Flag / Password

- **Password:** Retrieved from the `readme` file.

*Note: Avoid publishing the actual password in a public repository.*

## 6. What I Learned

- **`ls`** — Lists files and directories.
- **`cat`** — Displays the contents of a file.
- **SSH** — Provides secure remote access to a system.
- **Linux file navigation** — Files can be accessed using command-line tools when the appropriate permissions are available.

## 7. References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [SSH Manual](https://man.openbsd.org/ssh)