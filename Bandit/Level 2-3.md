# Bandit Level 2 → Level 3

## Challenge Information

- **Platform:** OverTheWire Bandit
- **Category:** Linux / Command Line
- **Difficulty:** Beginner
- **Tools Used:** SSH, Linux Terminal
- **Objective:** Find the password for Level 3 stored in a file named `--spaces in this filename--` in the home directory.

## 1. Challenge Description

The password for the next level is stored in a file whose name contains spaces and begins with hyphens. The goal is to access the file correctly and retrieve the password.

## 2. Initial Enumeration

List the files in the home directory.

```bash
ls
```

**Observations:**
- A file named `--spaces in this filename--` is present in the home directory.
- The filename contains spaces and begins with hyphens.

## 3. Analysis and Investigation

First, I tried reading the file using quotes:

```bash
cat "--spaces in this filename--"
```

**Mistake:** Quotes ensure the shell treats the filename as a single argument, but they do not prevent `cat` from interpreting a leading hyphen as an option.

The correct approach is to specify the file path using `./` and escape the spaces:

```bash
cat ./--spaces\ in\ this\ filename--
```

**Key Finding:** The `./` prefix makes the argument a relative file path, while the backslashes prevent the shell from splitting the filename at spaces.

## 4. Solution

1. Run `ls` to list the files in the home directory.
2. Identify the file named `--spaces in this filename--`.
3. Attempt to read it using quotes.
4. Use `cat ./--spaces\ in\ this\ filename--` to access the file correctly.
5. Copy the password displayed in the terminal.
6. Log in to the next level using SSH.

```bash
ssh -p 2220 bandit3@bandit.labs.overthewire.org
```

Enter the retrieved password when prompted.

## 5. Flag / Password

- **Password:** Retrieved from `--spaces in this filename--`.

*Note: Avoid publishing the actual password in a public repository.*

## 6. What I Learned

- **`ls`** — Lists files and directories.
- **`cat`** — Displays the contents of a file.
- **Spaces in filenames** — Quotes or backslash escaping prevent the shell from splitting a filename into multiple arguments.
- **Filenames beginning with hyphens** — Using `./` distinguishes a file path from a command option.
- **Shell parsing vs. command parsing** — The shell processes quoting and escaping before passing arguments to a command.

**Alternative:** You can also use `cat -- "--spaces in this filename--"` with GNU `cat`. Here, `--` marks the end of command options, and the quotes preserve the filename as one argument.

## 7. References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [GNU Coreutils — `cat`](https://www.gnu.org/software/coreutils/manual/html_node/cat-invocation.html)