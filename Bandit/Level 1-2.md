# Bandit Level 1 → Level 2

## Challenge Information

- **Platform:** OverTheWire Bandit
- **Category:** Linux / Command Line
- **Difficulty:** Beginner
- **Tools Used:** SSH, Linux Terminal
- **Objective:** Find the password for Level 2 stored in a file named `-` in the home directory.

## 1. Challenge Description

The password for the next level is stored in a file named `-`. Since this filename consists of a hyphen, it can be interpreted as an option by commands such as `cat`. The goal is to read the file correctly and retrieve the password.

## 2. Initial Enumeration

List the files in the home directory.

```bash
ls
```

**Observations:**
- A file named `-` is present in the home directory.

## 3. Analysis and Investigation

Initially, I tried the following command:

```bash
cat -*
```

**Mistake:** The `*` wildcard expands to matching filenames. If it expands to `-`, the `cat` command interprets the hyphen as an option rather than a regular filename.

The correct approach is to specify the file's path explicitly:

```bash
cat ./-
```

**Key Finding:** The `./` prefix identifies the file in the current directory, allowing `cat` to read the file named `-` correctly.

## 4. Solution

1. Run `ls` to list the files in the home directory.
2. Identify the file named `-`.
3. Execute `cat ./-` to display its contents.
4. Copy the password displayed in the terminal.
5. Log in to the next level using SSH.

```bash
ssh -p 2220 bandit2@bandit.labs.overthewire.org
```

Enter the retrieved password when prompted.

## 5. Flag / Password

- **Password:** Retrieved from the file named `-`.

*Note: Avoid publishing the actual password in a public repository.*

## 6. What I Learned

- **`ls`** — Lists files and directories.
- **`cat`** — Displays the contents of a file.
- **`*` wildcard** — Matches filenames according to shell pattern rules.
- **`./`** — Refers to the current directory and helps disambiguate filenames beginning with a hyphen.
- **Command-line argument parsing** — Some filenames can be mistaken for command options.

**Alternative:** `cat -- -` also works with GNU `cat`, because `--` marks the end of command options.

## 7. References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [GNU Coreutils — `cat`](https://www.gnu.org/software/coreutils/manual/html_node/cat-invocation.html)