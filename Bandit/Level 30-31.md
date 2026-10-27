# Bandit Level 30 → Level 31

## Challenge Information

- **Platform:** OverTheWire Bandit
- **Category:** Git / Version Control
- **Difficulty:** Beginner
- **Tools Used:** Git, SSH, Linux Terminal
- **Objective:** Find the password for `bandit31` hidden in the Git repository.

## 1. Challenge Description

The challenge provides a Git repository accessible over SSH on port `2220`. The goal is to clone the repository onto the local machine and investigate its contents to find the next level's password.

## 2. Initial Enumeration

### Step 1: Clone the Repository

I cloned the repository from my local Linux Mint machine using:

```bash
git clone ssh://bandit30-git@bandit.labs.overthewire.org:2220/home/bandit30-git/repo bandit30-repo
```

**Explanation:**
- `git clone` downloads a copy of a Git repository.
- `ssh://` specifies the SSH protocol.
- `:2220` specifies the SSH port.
- `bandit30-repo` is the local directory created for the repository.

### Step 2: Inspect the Files

```bash
cd bandit30-repo
ls
cat README.md
```

The repository contained a `README.md` file with the following message:

```text
just an epmty file... muahaha
```

The README did not contain the password directly.

## 3. Analysis and Investigation

### Step 1: Inspect the Commit History

```bash
git log
```

The history showed only one commit: the initial commit of `README.md`.

I also checked the available branches:

```bash
git branch -a
```

Only the `master` branch and its remote reference were present. There were no additional branches to investigate.

### Step 2: Inspect the Git Tag

Since the file, commit history, and branches did not reveal anything useful, I checked the repository's tags:

```bash
git tag
```

The output revealed a tag named `secret`.

**Why this matters:** Git tags are references to specific points in a repository's history. They can contain useful clues even when the current files do not reveal the required information.

### Step 3: Read the Tag's Contents

I inspected the tag using:

```bash
git show secret
```

This displayed the password for the next level.

Unlike the earlier levels, the password was not hidden in a file or an earlier commit. It was revealed by inspecting the tag.

## 4. Solution

The essential commands were:

```bash
git tag
git show secret
```

The first command lists the repository's tags, and the second displays the object referenced by the `secret` tag.

I then used the retrieved password to connect to the next level:

```bash
ssh -p 2220 bandit31@bandit.labs.overthewire.org
```

After entering the password, the login succeeded, confirming that I had completed the challenge.

## 5. What I Learned

- **Git tags:** Tags provide named references to specific objects in a repository.
- **Repository enumeration:** When files, branches, and commit history provide no useful information, tags are another place to investigate.
- **`git tag`:** Lists the tags available in the local repository.
- **`git show`:** Displays information about commits, tags, and other Git objects.
- **Systematic investigation:** Git repositories contain more than just the current files and commit history.

## 6. References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [Git Documentation — `git tag`](https://git-scm.com/docs/git-tag)
- [Git Documentation — `git show`](https://git-scm.com/docs/git-show)

## Key Takeaway

**When a Git repository appears empty, inspect its metadata too.** Tags, branches, and commit history can reveal information that is not visible in the current working tree.

*Note: The actual password has been omitted from this write-up.*