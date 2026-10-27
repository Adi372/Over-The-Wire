# Bandit Level 29 → Level 30

## Challenge Information

- **Platform:** OverTheWire Bandit
- **Category:** Git, Branch Enumeration, Information Disclosure
- **Difficulty:** Easy
- **Tools Used:** `git clone`, `git branch`, `git show`, `ssh`
- **Objective:** Explore the repository's branches to find the password for `bandit30`.

## 1. Challenge Description

The challenge provides a Git repository accessible through SSH on port `2220`. The password for `bandit29-git` is the same as the password for `bandit29`.

The objective is to clone the repository onto my local machine and retrieve the password for the next level.

After inspecting the README, I found that the password was replaced with a placeholder. The commit history did not immediately reveal the password, so I investigated the repository's available branches.

## 2. Initial Enumeration

I cloned the repository from my local machine using:

```bash
git clone ssh://bandit29-git@bandit.labs.overthewire.org:2220/home/bandit29-git/repo bandit29-repo
```

I used `bandit29-repo` as the destination directory to keep it separate from the existing repositories.

Next, I entered the directory and listed its contents.

```bash
cd bandit29-repo
ls
```

The repository contained:

```text
README.md
```

I inspected the README:

```bash
cat README.md
```

It contained notes for level 30, but the password field displayed:

```text
password: <no passwords in production!>
```

This indicated that the password was not available in the current version of the README.

## 3. Analysis and Investigation

### Step 1: Inspect the commit history

I checked the repository's commit history.

```bash
git log
```

The output showed two commits:

- `9be4958` — `fix username`
- `03a5aba` — `initial commit of README.md`

I inspected the latest commit:

```bash
git show 9be4958108e5abc1e37562694a53259b1015271e
```

The diff showed that the username had been changed from `bandit29` to `bandit30`. However, the password remained a placeholder.

This meant that the latest commit did not provide the password directly.

### Step 2: List all available branches

I checked the local and remote branches.

```bash
git branch -a
```

The output showed:

```text
* master
  remotes/origin/HEAD -> origin/master
  remotes/origin/dev
  remotes/origin/master
  remotes/origin/sploits-dev
```

The important discovery was that the repository contained additional remote branches, including `dev` and `spl oits-dev` (shown in the terminal as `sploits-dev`).

Git branches can contain different versions of a project's files. Therefore, a file that does not contain useful information on `master` might contain it on another branch.

### Step 3: Inspect the `dev` branch

Instead of switching branches, I displayed the README directly from the remote `dev` branch.

```bash
git show origin/dev:README.md
```

The output showed the README from `origin/dev`, including the password field.

This revealed the password for `bandit30`, which I used to log in to the next level.

## 4. Solution

### Step 1: Retrieve the README from the remote branch

The key command was:

```bash
git show origin/dev:README.md
```

Explanation:

- `git show` displays a Git object or a file from a commit or branch.
- `origin/dev` identifies the remote-tracking branch named `dev`.
- `:README.md` specifies the file to display from that branch.

The command revealed the password that was missing from the README on `master`.

### Step 2: Log in as `bandit30`

After retrieving the password, I connected to the next level:

```bash
ssh -p 2220 bandit30@bandit.labs.overthewire.org
```

After entering the recovered password, I successfully logged in as `bandit30`.

## 5. What I Learned

- **Git branches:** Different branches can contain different versions of a project's files.
- **Remote-tracking branches:** Branches such as `origin/dev` represent branches tracked from a remote repository.
- **`git branch -a`:** Lists local branches and remote-tracking branches.
- **`git show`:** Can display a file directly from a particular branch without checking it out.
- **Information disclosure:** Important information can remain accessible in another branch even when it is absent from the default branch.
- **Repository enumeration:** Checking branches is an important step when the current files and commit history do not reveal the required information.

## 6. References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [Git — Branches in a Nutshell](https://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell)
- [Git — Viewing the Commit History](https://git-scm.com/book/en/v2/Git-Basics-Viewing-the-Commit-History)

## Key Takeaway

This level demonstrates how a Git repository's branches can contain information that is not available on its default branch. By listing the available branches and inspecting `origin/dev`, I recovered the password needed to access `bandit30`.

The main lesson is to inspect the repository's branches as well as its files and commit history when investigating a Git-based challenge.