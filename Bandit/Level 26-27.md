# Bandit Level 26 → Level 27

## Challenge Information

- **Platform:** OverTheWire Bandit
- **Category:** Linux, SUID, Privilege Escalation
- **Difficulty:** Easy
- **Tools Used:** `ls`, `file`, `find`, `bandit27-do`
- **Objective:** Find and use the provided SUID executable to retrieve the password for `bandit27`.

## 1. Challenge Description

After obtaining an interactive shell as `bandit26`, the objective is to find the password for the next level, `bandit27`.

The challenge hints at using commands such as `ls` and `cat`. I began by inspecting the files available in the home directory and identifying their types and permissions.

## 2. Initial Enumeration

I listed the contents of the home directory.

```bash
ls
```

The output showed two files:

```text
bandit27-do
text.txt
```

Next, I checked their file types.

```bash
file . ./*
```

The relevant output identified `bandit27-do` as a **32-bit ELF executable** and `text.txt` as an ASCII text file.

The executable's name suggested that it could perform an operation as `bandit27`. I investigated it further rather than assuming its purpose from the filename alone.

## 3. Analysis and Investigation

I also searched for files owned by `bandit27`.

```bash
find / -user bandit27 2>/dev/null
```

Explanation:

- `find /` searches from the filesystem root.
- `-user bandit27` matches files owned by `bandit27`.
- `2>/dev/null` suppresses error messages, such as permission-denied errors.

The command returned many entries under `/sys/fs/cgroup`, which were not useful for retrieving the password. This demonstrated that searching by file owner can produce noisy results, so examining the provided executable was the more relevant next step.

## 4. Solution

### Step 1: Inspect the SUID executable

The executable available in the home directory was:

```text
bandit27-do
```

Its purpose is to execute a command with the privileges of `bandit27`. This is the important security property of a SUID executable: when configured with the set-user-ID permission, it can run with the effective user ID of its owner.

### Step 2: Read the next level's password

The intended approach is to use `bandit27-do` to execute `cat` on the password file for `bandit27`.

```bash
./bandit27-do cat /etc/bandit_pass/bandit27
```

Explanation:

- `./bandit27-do` runs the executable from the current directory.
- `cat` is the command passed to the executable.
- `/etc/bandit_pass/bandit27` is the password file for the next level.

The command prints the password to the terminal. I used that password to log in as `bandit27`.

**Note:** The supplied terminal output confirms that `bandit27-do` exists, but it does not include the result of executing it. Run the command above to verify the password retrieval before marking this step as completed in your write-up.

## 5. What I Learned

- **SUID executables:** Can run with the effective user ID of the executable's owner when the SUID bit is enabled.
- **File identification:** `file` helps identify whether a file is a script, text file, or compiled executable.
- **Filesystem enumeration:** `find` can locate files by owner, although broad searches may return irrelevant results.
- **Command execution:** Some executables accept a command and its arguments, allowing that command to run with different privileges.
- **Least privilege:** SUID programs must be designed carefully because unsafe command execution can expose files that the current user could not normally read.

## 6. References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [`find` Manual Page](https://man7.org/linux/man-pages/man1/find.1.html)
- [`file` Manual Page](https://man7.org/linux/man-pages/man1/file.1.html)
- [`cat` Manual Page](https://man7.org/linux/man-pages/man1/cat.1.html)

## Key Takeaway

This level focuses on identifying a provided executable that can perform operations with another user's privileges. By inspecting `bandit27-do` and using it to read the next level's password file, the intended solution is to move from `bandit26` to `bandit27`.