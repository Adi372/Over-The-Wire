# Bandit Level 3 → Level 4

## Challenge Information

- **Platform:** OverTheWire Bandit
- **Category:** Linux / Command Line
- **Difficulty:** Beginner
- **Tools Used:** SSH, Linux Terminal
- **Objective:** Find the password for Level 4 stored in a hidden file inside the `inhere` directory.

## 1. Challenge Description

The password for the next level is stored in a hidden file inside the `inhere` directory. The goal is to locate the hidden file and read its contents to retrieve the password.

## 2. Initial Enumeration

List the files and directories in the home directory.

```bash
ls
```

**Observations:**
- A directory named `inhere` is present in the home directory.

Navigate into the directory:

```bash
cd inhere/
```

## 3. Analysis and Investigation

Initially, running `ls` inside the directory did not reveal the hidden file.

```bash
ls
```

To display all files, including hidden files, use:

```bash
ls -la
```

**Observations:**
- A hidden file named `...Hiding-From-You` is present.
- The filename begins with a dot, so it is hidden by default.

Read the file using:

```bash
cat ./...Hiding-From-You
```

**Key Finding:** The hidden file contains the password required to access Level 4.

## 4. Solution

1. Run `ls` to locate the `inhere` directory.
2. Execute `cd inhere/` to enter the directory.
3. Run `ls -la` to display all files, including hidden files.
4. Identify the file named `...Hiding-From-You`.
5. Execute `cat ./...Hiding-From-You` to display its contents.
6. Copy the retrieved password and log in to the next level.

```bash
ssh -p 2220 bandit4@bandit.labs.overthewire.org
```

Enter the retrieved password when prompted.

## 5. Flag / Password

- **Password:** Retrieved from `...Hiding-From-You`.

*Note: Avoid publishing the actual password in a public repository.*

## 6. What I Learned

- **`cd`** — Changes the current working directory.
- **`ls -la`** — Lists all directory entries, including hidden files, with detailed information.
- **Hidden files** — In Linux, filenames beginning with `.` are hidden from ordinary directory listings by default.
- **`cat`** — Displays the contents of a file.
- **Relative paths** — The `./` prefix specifies a file in the current directory.

## 7. References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [GNU Coreutils — `ls`](https://www.gnu.org/software/coreutils/manual/html_node/ls-invocation.html)