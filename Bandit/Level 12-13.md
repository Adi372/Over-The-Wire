# Bandit Level 12 → Level 13

## Challenge Information

- **Platform:** OverTheWire Bandit
- **Category:** Linux / File Analysis / Compression
- **Difficulty:** Beginner
- **Tools Used:** SSH, Linux Terminal, `mktemp`, `xxd`, `file`, `gzip`, `bzip2`, `tar`
- **Objective:** Recover the password from `data.txt`, which contains a hexdump of a repeatedly compressed file.

## 1. Challenge Description

The password for the next level is stored in a file that has been compressed multiple times using different compression methods and archive formats. The original file is represented as a hexdump in `data.txt`.

The goal is to reverse the hexdump, identify each file format, and repeatedly decompress or extract the contents until the password becomes readable.

## 2. Initial Setup

Create a temporary directory to work safely with the files and copy `data.txt` into it.

```bash
mktemp -d
cp data.txt /tmp/tmp.hgJEsaGAKG
cd /tmp/tmp.hgJEsaGAKG
```

**Observations:**
- A temporary working directory keeps the extracted files separate from the original challenge files.
- The filename returned by `mktemp -d` will be different each time, so use your own generated path.

## 3. Analysis and Investigation

### Step 1: Reverse the hexdump

Convert the hexadecimal representation back into binary data.

```bash
xxd -r data.txt > decoded
file decoded
```

**Finding:** The `file` command identifies the actual format, which determines the appropriate decompression method.

### Step 2: Decompress the gzip file

Rename the file with a `.gz` extension and decompress it.

```bash
mv decoded data.gz
gzip -d data.gz
file data
```

### Step 3: Decompress the bzip2 file

The resulting file is compressed using bzip2.

```bash
bzip2 -d data
file data.out
```

**Observation:** The output was named `data.out` because the decompressor could not determine the original filename.

Rename it to use the gzip extension:

```bash
mv data.out data.gz
gzip -d data.gz
file data
```

### Step 4: Extract the tar archives

The next file is a tar archive. Extract it and inspect the resulting files.

```bash
tar -xf data
file data5.bin
```

Continue extracting the nested archives as indicated by the file names and the output of `file`.

```bash
tar -xf data5.bin
tar -xf data6.bin
tar -xf data8.bin
file data8.bin
```

### Step 5: Decompress the final gzip file

If `file` identifies `data8.bin` as gzip-compressed data, rename it and decompress it.

```bash
mv data8.bin data.gz
gzip -d data.gz
file data
```

**Key Finding:** The repeated process of identifying, decompressing, and extracting the file eventually produces readable ASCII text.

## 4. Solution

1. Create a temporary working directory using `mktemp -d`.
2. Copy `data.txt` into the directory.
3. Reverse the hexdump using `xxd -r`.
4. Use `file` to identify the format at every stage.
5. Decompress gzip and bzip2 files using the appropriate commands.
6. Extract tar archives using `tar -xf`.
7. Repeat the process until the final file contains readable text.
8. Display the contents to retrieve the password.

```bash
cat data
```

Log in to Level 13 using SSH:

```bash
ssh -p 2220 bandit13@bandit.labs.overthewire.org
```

Enter the retrieved password when prompted.

## 5. Flag / Password

- **Password:** Retrieved from the final readable contents of `data`.

*Note: Keep the password private and avoid publishing it in your README.*

## 6. Mistakes and Corrections

- **Incorrect command:** `bzip -d data` failed because the correct decompression command is `bzip2 -d data`.
- **Unexpected output filename:** `bzip2` produced `data.out` because the original filename could not be determined.
- **Interrupted decompression:** Pressing `Ctrl+C` stopped the `gzip` operation. The correct input file needed to be identified and renamed before retrying.
- **Overwrite prompt:** When `gzip` asked whether to overwrite an existing file, answering `y` allowed the operation to continue. Before overwriting, verify that the existing file is not needed.

## 7. What I Learned

- **`mktemp -d`** — Creates a temporary directory with a unique name.
- **`cp`** — Copies files.
- **`xxd -r`** — Reverses a hexdump into binary data.
- **`file`** — Identifies a file's type by examining its contents.
- **`mv`** — Moves or renames files.
- **`gzip -d`** — Decompresses gzip files.
- **`bzip2 -d`** — Decompresses bzip2 files.
- **`tar -xf`** — Extracts tar archives.
- **File signatures** — The actual format of a file matters more than its filename or extension.

**Important lesson:** Use `file` after each extraction or decompression. Do not assume the next command based only on the filename.

## 8. References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [Linux `xxd` manual](https://manpages.debian.org/xxd)
- [GNU gzip manual](https://www.gnu.org/software/gzip/manual/gzip.html)
- [GNU tar manual](https://www.gnu.org/software/tar/manual/tar.html)