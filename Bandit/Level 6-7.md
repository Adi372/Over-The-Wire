# Bandit Level 6 → Level 7

## Level Goal
Find the password for Level 7 stored somewhere on the server in a file owned by user `bandit7`, group `bandit6`, and exactly 33 bytes in size.

## Commands Used
```bash
find / -type f -size 33c -user bandit7 -group bandit6
find / -type f -size 33c -user bandit7 -group bandit6 2>/dev/null
cat /var/lib/dpkg/info/bandit7.password
```

## Steps
1. Use `find /` to search the entire server.
2. Apply the conditions `-type f`, `-size 33c`, `-user bandit7`, and `-group bandit6`.
3. Use `2>/dev/null` to hide permission-denied and other error messages.
4. Locate the matching file at `/var/lib/dpkg/info/bandit7.password`.
5. Use `cat` with the absolute path to read the password.

## Mistakes Made
- **Used `find -la`:** `-la` is an option for `ls`, not `find`.
- **Searched only the home directory:** `find .` searches from the current directory, but this password could be anywhere on the server. Using `find /` searches from the root directory.
- **Ignored permission errors initially:** Searching the whole server produced many permission-denied messages. Adding `2>/dev/null` hides these errors.
- **Used an incorrect relative path:** `./var/lib/...` points to a `var` directory inside the current directory, not `/var`. Use the absolute path `/var/lib/dpkg/info/bandit7.password`.

## Key Learning
- **`find /`** — Searches the filesystem starting at the root directory.
- **`-size 33c`** — Finds files exactly 33 bytes in size.
- **`-user` and `-group`** — Filter files by owner and group.
- **`2>/dev/null`** — Redirects error output to `/dev/null`.
- **Absolute path (`/var/...`)** — Specifies a location starting from the filesystem root.

## Next Level Login
```bash
ssh -p 2220 bandit7@bandit.labs.overthewire.org
```

**Note:** Use absolute paths when accessing files outside your current directory.