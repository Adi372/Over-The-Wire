# Bandit Level 12 → Level 13

## Level Goal
Recover the password from `data.txt`, which is a hexdump of a repeatedly compressed file.

## Commands Used
```bash
mktemp -d
cp data.txt /tmp/tmp.hgJEsaGAKG
cd /tmp/tmp.hgJEsaGAKG
xxd -r data.txt > decoded
file decoded
mv decoded data.gz
gzip -d data.gz
file data
bzip2 -d data
file data.out
mv data.out data.gz
gzip -d data.gz
file data
tar -xf data
file data5.bin
tar -xf data5.bin
tar -xf data6.bin
tar -xf data8.bin
file data8.bin
mv data8.bin data.gz
gzip -d data.gz
file data
cat data
```

## Steps
1. Created a temporary directory using `mktemp -d` and copied `data.txt` into it.
2. Used `xxd -r` to convert the hexdump back into binary data.
3. Used `file` to identify the file type at each stage.
4. Renamed files with `mv` when needed so compression tools could process them.
5. Decompressed the files using `gzip -d` and `bzip2 -d`, and extracted archives using `tar -xf`.
6. Repeated the process until `file` identified the final file as `ASCII text`.
7. Used `cat data` to display the text containing the next level's password.

## Mistakes Made
- `bzip -d data` failed because the correct command is `bzip2 -d data`.
- `bzip2` produced `data.out` because it couldn't determine the original filename.
- `gzip -d data` was interrupted with `Ctrl+C`; the correct file was `data.out`, which needed renaming first.
- When `gzip` asked whether to overwrite an existing `data` file, answered `y` to continue.

## Key Learning
- `mktemp -d`: Creates a temporary directory with a unique name.
- `cp`: Copies files.
- `xxd -r`: Reverses a hexdump into binary data.
- `file`: Identifies a file's type.
- `mv`: Renames or moves files.
- `gzip -d` / `bzip2 -d`: Decompresses compressed files.
- `tar -xf`: Extracts a tar archive.

**Important:** File extensions can be misleading. Use `file` to identify the actual format before choosing the next command.

## Next Level Login
```bash
ssh -p 2220 bandit13@bandit.labs.overthewire.org
```

**Note:** Save the password privately. Do not include it in the README.
