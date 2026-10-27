# Bandit Level 28 → Level 29

## Challenge Information

- **Platform:** OverTheWire Bandit
- **Category:** Git, Version Control, Information Disclosure
- **Difficulty:** Easy
- **Tools Used:** `git clone`, `git log`, `git show`, `cat`, `ssh`
- **Objective:** Inspect the Git repository's commit history to recover a password that was removed from the latest version of a file.

## 1. Challenge Description

The challenge provides a Git repository accessible through SSH on port `2220`. The password for `bandit28-git` is the same as the password for `bandit28`.

The objective is to clone the repository onto my local machine and find the password for `bandit29`.

Unlike the previous level, the password is not visible in the current version of the README. I needed to investigate the repository's commit history to find information that had been removed.

## 2. Initial Enumeration

I first logged in as `bandit28` to confirm that my credentials worked.

```bash
ssh -p 2220 bandit28@bandit.labs.overthewire.org
```

After exiting the SSH session, I cloned the repository from my local machine.

```bash
git clone ssh://bandit28-git@bandit.labs.overthewire.org:2220/home/bandit28-git/repo bandit28-repo
```

I used `bandit28-repo` as the destination directory because a directory named `repo` already existed in my `Documents` folder.

The clone completed successfully. I then entered the new directory and listed its contents.

```bash
cd bandit28-repo
ls
```

The repository contained:

```text
README.md
```

I inspected the file:

```bash
cat README.md
```

The README contained notes for level 29, but the password field had been replaced with `xxxxxxxxxx`.

This suggested that the password had been removed from the latest version of the file, so I investigated the Git history.

## 3. Analysis and Investigation

### Step 1: Inspect the commit history

I ran:

```bash
git log
```

The output showed three commits:

- `801b85f` — `fix info leak`
- `df10da1` — `add missing data`
- `388bcd3` — `initial commit of README.md`

The commit messages suggested that information had been added and then removed. Git stores previous versions of tracked files in its commit history, so modifying the latest README does not necessarily remove the earlier content from existing commits.

### Step 2: Inspect the earlier commit

I examined the commit named `add missing data`.

```bash
git show df10da19c2625f7c59e071e16030bd587d8c409d
```

The output displayed the changes introduced by that commit. The diff showed that the password field had changed from a placeholder to the actual password.

The relevant part of the diff looked like this:

```diff
- - password: <TBD>
+ - password: [redacted]
```

This revealed that the earlier commit contained the password that was later removed.

### Step 3: Display the previous version of the README

To inspect the complete README from that specific commit, I ran:

```bash
git show df10da19c2625f7c59e071e16030bd587d8c409d:README.md
```

The command displayed the README as it existed in that commit, including the original password field.

This confirmed that the information was still recoverable from the repository's history.

## 4. Solution

### Step 1: Retrieve the password from Git history

The key command was:

```bash
git show df10da19c2625f7c59e071e16030bd587d8c409d:README.md
```

The syntax is:

- `git show` displays information about a commit or Git object.
- The commit hash identifies the specific historical version.
- `:README.md` specifies the file to display from that commit.

I used the recovered password to connect as `bandit29`.

### Step 2: Log in to the next level

```bash
ssh -p 2220 bandit29@bandit.labs.overthewire.org
```

After entering the recovered password, I successfully logged in as `bandit29`.

## 5. What I Learned

- **Git history:** Git retains previous versions of tracked files in reachable commits.
- **`git log`:** Lists commits and helps identify potentially relevant changes.
- **`git show`:** Displays commit details and file changes.
- **Git diffs:** The `+` and `-` lines show content added and removed by a commit.
- **Historical file inspection:** `git show COMMIT:FILE` retrieves a file from a specific commit.
- **Information disclosure:** Removing a secret from the latest version of a file does not automatically remove it from Git history.
- **Secure secret management:** Passwords, API keys, and tokens should not be committed to repositories. If exposed, they should be revoked or rotated, and sensitive history should be handled appropriately.

## 6. References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [Git — Getting a Git Repository](https://git-scm.com/book/en/v2/Git-Basics-Getting-a-Git-Repository)
- [Git — Viewing the Commit History](https://git-scm.com/book/en/v2/Git-Basics-Viewing-the-Commit-History)

## Key Takeaway

This level demonstrates that removing sensitive information from the latest version of a file does not necessarily remove it from the repository. By inspecting the commit history with `git log` and `git show`, I recovered the password that had been removed from the README.

The main lesson is to check version history when investigating a Git repository and to avoid committing secrets in the first place.