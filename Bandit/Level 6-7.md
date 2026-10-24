# Bandit Level 6 → Level 7

## Challenge Information

- **Platform:** OverTheWire Bandit
- **Category:** Linux / Command Line
- **Difficulty:** Beginner
- **Tools Used:** SSH, Linux Terminal, `find`
- **Objective:** Find the password for Level 7 in a file somewhere on the server that is owned by user `bandit7`, belongs to group `bandit6`, and is exactly 33 bytes in size.

## 1. Challenge Description

The password for the next level is stored somewhere on the server. The target file must satisfy three conditions:

- Owned by user `bandit7`.
- Owned by group `bandit6`.
- Exactly 33 bytes in size.

The goal is to search the filesystem, identify the matching file, and read its contents.

## 2. Initial Enumeration

Initially, I tried using an option that belongs to `ls`:

```bash
find -la
```

**Mistake:** The `-la` option is used with `ls` to list files, including hidden files, in a detailed format. It is not a valid equivalent option for `find`.

Since the password could be anywhere on the server, the search must start from the filesystem root.

## 3. Analysis and Investigation

Run `find` with the required conditions:

```bash
find / -type f -size 33c -user bandit7 -group bandit6
```

**Observations:**
- The search starts at `/`, the filesystem root.
- The command may produce permission-denied messages while traversing protected directories.

To suppress error messages, use:

```bash
find / -type f -size 33c -user bandit7 -group bandit6 2>/dev/null
```

**Key Finding:** The matching file is located at `/var/lib/dpkg/info/bandit7.password`.

## 4. Solution

1. Run `find /` to search the filesystem from the root directory.
2. Use `-type f` to search only regular files.
3. Apply `-size 33c` to match files exactly 33 bytes in size.
4. Filter by owner using `-user bandit7`.
5. Filter by group using `-group bandit6`.
6. Add `2>/dev/null` to suppress error messages.
7. Read the matching file using its absolute path.

```bash
cat /var/lib/dpkg/info/bandit7.password
```

Copy the password and log in to Level 7:

```bash
ssh -p 2220 bandit7@bandit.labs.overthewire.org
```

## 5. Flag / Password

- **Password:** Retrieved from `/var/lib/dpkg/info/bandit7.password`.

*Note: Avoid publishing the actual password in a public repository.*

## 6. What I Learned

- **`find /`** — Searches the filesystem starting from the root directory.
- **`-type f`** — Matches regular files.
- **`-size 33c`** — Matches files exactly 33 bytes in size; `c` means bytes.
- **`-user` and `-group`** — Filter files by their owner and group.
- **`2>/dev/null`** — Redirects standard error (file descriptor 2) to `/dev/null`, suppressing error messages.
- **Absolute paths** — A path beginning with `/` specifies a location relative to the filesystem root.
- **`find` vs. `ls`** — Different commands have different options and purposes.

## 7. Mistakes and Corrections

- **Used `find -la`:** This is not the correct syntax for searching files. Use `find` with search conditions.
- **Searched only the home directory:** `find .` searches from the current directory. Use `find /` when the target could be elsewhere on the server.
- **Ignored permission errors initially:** Adding `2>/dev/null` suppresses error messages without changing the search criteria.
- **Used an incorrect relative path:** `./var/lib/...` refers to a path inside the current directory. Use `/var/lib/...` for the absolute path.

## 8. References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [Linux `find` command manual](https://man7.org/linux/man-pages/man1/find.1.html)
- [Linux pathname conventions](https://man7.org/linux/man-pages/man7/pathname.7.html)