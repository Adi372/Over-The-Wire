# Bandit Level 32 → Level 33

## Challenge Information

- **Platform:** OverTheWire Bandit
- **Category:** Linux / Shell Escaping
- **Difficulty:** Beginner
- **Tools Used:** SSH, Linux Shell (`sh`)
- **Objective:** Escape the restricted uppercase shell and retrieve the password for the next level.

## 1. Challenge Description

After completing the Git challenges, this level introduces a different problem. When logging in as `bandit32`, the user is placed inside a custom shell called the **UPPERCASE SHELL**.

The shell changes the commands entered by the user to uppercase, making ordinary lowercase Linux commands difficult to execute as expected.

The goal is to escape this restricted shell and execute commands in a normal shell environment.

## 2. Initial Enumeration

### Step 1: Connect to Bandit Level 32

I connected to the remote server using SSH:

```bash
ssh -p 2220 bandit32@bandit.labs.overthewire.org
```

After authentication, the server displayed:

```text
WELCOME TO THE UPPERCASE SHELL
>>
```

This indicated that I was not placed in a normal interactive shell.

### Step 2: Test the Shell

I entered:

```text
$0
```

The shell displayed a new prompt, and I tested the environment:

```bash
whoami
ls
```

The output showed:

```text
bandit33
uppershell
```

This indicated that I had successfully escaped the restricted shell and was now operating as `bandit33`.

## 3. Analysis and Investigation

### Step 1: Understand the `$0` Variable

In a shell, `$0` represents the name or invocation used to start the current shell or script.

Entering `$0` in the uppercase shell caused it to launch the shell represented by that variable. In this challenge, that provided a way to escape the restricted command-processing environment.

**Why did this work?**

The restricted shell transformed ordinary input into uppercase, but the special shell parameter `$0` provided a way to invoke the shell itself rather than entering a conventional command that would be converted to uppercase.

### Step 2: Verify the Escape

After entering `$0`, I ran:

```bash
whoami
```

The output was:

```text
bandit33
```

I also ran:

```bash
ls
```

The output showed the `uppershell` file. These commands confirmed that the shell escape had worked.

## 4. Solution

Once inside the new shell, I retrieved the next level's password using:

```bash
cat /etc/bandit_pass/bandit33
```

The command displayed the password for `bandit33`.

I then exited the session and tested the retrieved password by connecting to the next account:

```bash
ssh -p 2220 bandit33@bandit.labs.overthewire.org
```

The login succeeded, confirming that the challenge was completed.

*The actual password has been omitted from this public write-up.*

## 5. What I Learned

- **Restricted shells:** Some environments limit or modify command execution.
- **Shell variables:** `$0` refers to the shell or script's invocation name.
- **Shell escaping:** Understanding shell behavior can help identify ways out of a restricted command environment.
- **Verification:** Running `whoami` after escaping confirms which account is active.
- **Linux shell behavior:** Shells are programs with their own execution rules, not merely command prompts.

## 6. References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [Bash Reference Manual — Special Parameters](https://www.gnu.org/software/bash/manual/html_node/Special-Parameters.html)

## Key Takeaway

**Understanding how a shell works can be more important than knowing individual commands.** In this challenge, `$0` provided a way to escape the uppercase shell and execute commands in a normal shell environment.