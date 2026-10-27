# Bandit Level 27 → Level 28

## Challenge Information

- **Platform:** OverTheWire Bandit
- **Category:** Git, Repository Enumeration
- **Difficulty:** Easy
- **Tools Used:** `git`, `ls`, `cat`, `ssh`
- **Objective:** Clone the provided Git repository on the local machine and retrieve the password for `bandit28`.

## 1. Challenge Description

The challenge provides a Git repository accessible through SSH:

```text
ssh://bandit27-git@bandit.labs.overthewire.org/home/bandit27-git/repo
```

The repository uses port `2220`, and the password for `bandit27-git` is the same as the password for `bandit27`.

The objective is to clone the repository onto my local machine, inspect its contents, and find the password for the next level.

## 2. Initial Enumeration

I started from my local machine, Linux Mint, and cloned the repository using:

```bash
git clone ssh://bandit27-git@bandit.labs.overthewire.org:2220/home/bandit27-git/repo
```

Explanation:

- `git clone` downloads a copy of a Git repository.
- `ssh://` specifies SSH as the transport protocol.
- `bandit27-git` is the repository username.
- `bandit.labs.overthewire.org` is the remote server.
- `:2220` specifies the SSH port.
- `/home/bandit27-git/repo` is the repository path.

Git prompted me for the password of `bandit27-git`. After entering the password for `bandit27`, the repository was cloned successfully into a local directory named `repo`.

## 3. Analysis and Investigation

After cloning the repository, I entered the directory and listed its contents.

```bash
cd repo
ls
```

The output showed a file named:

```text
README
```

Since the repository contained a README file, I inspected it using:

```bash
cat README
```

The file contained a message indicating that the password for the next level was stored there.

This was a straightforward repository-enumeration challenge: the required information was available directly in the repository's working directory, so there was no need to inspect Git history or run additional commands.

## 4. Solution

### Step 1: Read the README file

I used the following command from inside the cloned repository:

```bash
cat README
```

The command displayed the password for `bandit28`.

I used that password to log in to the next level.

### Step 2: Connect as `bandit28`

```bash
ssh -p 2220 bandit28@bandit.labs.overthewire.org
```

Here, `-p 2220` specifies the SSH port, and `bandit28` is the account I wanted to access.

After entering the retrieved password, I successfully logged in as `bandit28`.

## 5. What I Learned

- **Git repositories:** A repository can contain source files, documentation, configuration, and other information.
- **`git clone`:** Creates a local copy of a remote repository.
- **SSH repository URLs:** Git can use SSH for authentication and repository access.
- **Repository enumeration:** Listing and reading files is an important first step when investigating an unfamiliar repository.
- **`cat`:** Displays the contents of a text file in the terminal.
- **Local and remote environments:** The challenge specifically requires cloning the repository on the local machine rather than on the OverTheWire server.

## 6. References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [Installing Git — Pro Git Book](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
- [Git from the Bottom Up](https://jwiegley.github.io/git-from-the-bottom-up/)

## Key Takeaway

This level demonstrates how to access and inspect a remote Git repository using SSH. By cloning the repository locally and reading its README file, I retrieved the password required to access `bandit28`.

The main lesson is to enumerate repository contents before assuming that a challenge requires more complex Git operations.

