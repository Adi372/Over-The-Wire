# Bandit Level 10 → Level 11

## Challenge Information

- **Platform:** OverTheWire Bandit
- **Category:** Linux / Encoding
- **Difficulty:** Beginner
- **Tools Used:** SSH, Linux Terminal, `base64`
- **Objective:** Decode the Base64-encoded data in `data.txt` to retrieve the password for the next level.

## 1. Challenge Description

The password for the next level is stored in `data.txt` as Base64-encoded data. The goal is to decode the content and retrieve the original readable text.

## 2. Initial Enumeration

List the files in the home directory.

```bash
ls
```

**Observations:**
- The file `data.txt` is present in the home directory.
- Its contents are encoded rather than directly readable as the password.

## 3. Analysis and Investigation

Use the `base64` command with the `-d` option to decode the file's contents.

```bash
cat data.txt | base64 -d
```

**Observations:**
- `cat data.txt` reads the encoded content.
- The pipe (`|`) passes that content to `base64 -d`.
- The decoded output reveals the password for the next level.

**Key Finding:** Base64 decoding converts the encoded representation back into the original data.

## 4. Solution

1. Run `ls` to confirm that `data.txt` exists.
2. Execute `cat data.txt` to read the encoded content.
3. Pipe the output into `base64 -d`.
4. Copy the decoded password.
5. Log in to Level 11 using SSH.

```bash
cat data.txt | base64 -d
```

Log in using SSH:

```bash
ssh -p 2220 bandit11@bandit.labs.overthewire.org
```

Enter the retrieved password when prompted.

**Simpler alternative:**

```bash
base64 -d data.txt
```

This decodes the file directly without requiring `cat` or a pipe.

## 5. Flag / Password

- **Password:** Retrieved from the decoded contents of `data.txt`.

*Note: Keep the password private and avoid publishing it in your README.*

## 6. What I Learned

- **Base64** — An encoding scheme that represents binary data using printable characters; it is not encryption.
- **`base64 -d`** — Decodes Base64-encoded input.
- **Pipe (`|`)** — Passes one command's output to another command as input.
- **Encoding vs. encryption** — Encoding changes data representation; it does not provide confidentiality.

## 7. References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [GNU Coreutils — `base64`](https://www.gnu.org/software/coreutils/manual/html_node/base64-invocation.html)