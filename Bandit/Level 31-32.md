# Bandit Level 31 → Level 32

## Challenge Information

- **Platform:** OverTheWire Bandit
- **Category:** Git / Version Control
- **Difficulty:** Beginner
- **Tools Used:** Git, SSH, Linux Terminal
- **Objective:** Push a specific file to the remote Git repository to retrieve the password for the next level.

## 1. Challenge Description

The challenge provides a Git repository that must be cloned onto the local machine. The README specifies the file to create, its required content, and the branch to which it must be pushed.

The requirements are:

- **File name:** `key.txt`
- **Content:** `May I come in?`
- **Branch:** `master`

## 2. Initial Enumeration

### Step 1: Clone the Repository

I cloned the repository from my local Linux Mint machine:

```bash
git clone ssh://bandit31-git@bandit.labs.overthewire.org:2220/home/bandit31-git/repo bandit31-repo
```

Then I entered the repository and inspected its files:

```bash
cd bandit31-repo
ls
cat README.md
```

The README provided the exact file name, content, and target branch.

### Step 2: Inspect `.gitignore`

I checked the repository's ignore rules:

```bash
cat .gitignore
```

The output was:

```text
*.txt
```

This rule tells Git to ignore files ending in `.txt`. Therefore, the required `key.txt` file would not normally be included when staging files.

## 3. Analysis and Investigation

### Step 1: Create the Required File

I created `key.txt` with the exact content specified in the README:

```bash
echo "May I come in?" > key.txt
```

I verified the contents:

```bash
cat key.txt
```

The output matched the challenge requirements.

### Step 2: Force Git to Stage the Ignored File

Because `.gitignore` excludes all `.txt` files, I used:

```bash
git add -f key.txt
```

**Why use `-f`?**

The `-f` or `--force` option allows Git to stage a file even when an ignore rule excludes it. This was necessary to include `key.txt` in the commit.

### Step 3: Commit the File

I committed the staged file:

```bash
git commit -m "Add key.txt"
```

The commit was created successfully on the `master` branch.

### Step 4: Push to the Remote Repository

I pushed the commit to the remote repository:

```bash
git push origin master
```

Git uploaded the commit, and the remote server ran its validation process. The server displayed the password for the next level.

The push was ultimately rejected with a `pre-receive hook declined` message. However, **the challenge was successfully solved** because the server-side validation printed the password before rejecting the push.

## 4. Solution

The essential commands were:

```bash
echo "May I come in?" > key.txt
git add -f key.txt
git commit -m "Add key.txt"
git push origin master
```

The remote repository's validation hook checked the pushed files and returned the next level's password.

The rejection message does not mean the solution failed. The server intentionally controls which commits it accepts, and the challenge's validation output confirms that the required conditions were met.

*The actual password has been omitted from this public write-up.*

## 5. What I Learned

- **`.gitignore`:** Specifies files Git should normally ignore.
- **`git add -f`:** Forces an ignored file to be staged.
- **`git commit`:** Records staged changes in the local repository.
- **`git push`:** Sends local commits to a remote repository.
- **Server-side Git hooks:** Can inspect or validate incoming pushes and reject commits that do not satisfy repository rules.
- **Command output matters:** A rejected push can still produce useful server-side output before the rejection occurs.

## 6. References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [Git Documentation — `git add`](https://git-scm.com/docs/git-add)
- [Git Documentation — `git push`](https://git-scm.com/docs/git-push)
- [Git Documentation — `gitignore`](https://git-scm.com/docs/gitignore)
- [Git Documentation — Git Hooks](https://git-scm.com/docs/githooks)

## Key Takeaway

**A file being ignored by Git does not prevent you from staging it explicitly.** In this challenge, `git add -f key.txt` allowed the required file to be committed and submitted for remote validation.