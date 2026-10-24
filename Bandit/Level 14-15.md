# Bandit Level 14 → Level 15

## Challenge Information

- **Platform:** OverTheWire Bandit
- **Category:** Linux, Networking, Client-Server Communication
- **Difficulty:** Beginner
- **Tools Used:** `cat`, `nc` (Netcat), SSH
- **Objective:** Submit the current level's password to a service listening on port `30000` on the local machine to retrieve the next password.

## 1. Challenge Description

The password for the next level can be obtained by submitting the current level's password to port `30000` on `localhost`.

Instead of logging into another account immediately, I needed to connect to a local network service and send it the password.

## 2. Reading the Current Password

I read the password for `bandit14` from its password file:

```bash
cat /etc/bandit_pass/bandit14
```

The file contains the password for the current level. I kept this credential private rather than including it in the write-up.

## 3. Sending the Password to Port 30000

I piped the password directly into Netcat:

```bash
cat /etc/bandit_pass/bandit14 | nc localhost 30000
```

### Command breakdown

- `cat /etc/bandit_pass/bandit14` reads the current password.
- `|` (pipe) sends the command's output to the next command as input.
- `nc` (Netcat) creates a network connection and transfers data.
- `localhost` refers to the same machine, typically through the loopback address `127.0.0.1`.
- `30000` is the port where the service is listening.

This command connects to the service and sends the password without requiring me to type it manually.

## 4. Analyzing the Response

The service responded with:

```text
Correct!
```

It then returned the password for `bandit15`.

This confirmed that the submitted password was valid and that the service had accepted it.

## 5. Key Learnings

- **`localhost`:** Refers to the local machine, usually through the loopback interface.
- **Port:** Identifies a particular communication endpoint used by a service or application.
- **Netcat (`nc`):** Connects to network services and sends or receives data.
- **Pipe (`|`):** Redirects one command's standard output to another command's standard input.
- **Client-server communication:** A client connects to a service, sends a request, and receives a response.
- **Port 30000:** The port used by this challenge's service to receive and validate the submitted password.

## 6. Next Level Login

After retrieving the password, the next step is to connect as `bandit15`:

```bash
ssh -p 2220 bandit15@bandit.labs.overthewire.org
```

Enter the password obtained from the service when prompted.

## 7. References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [`nc` (Netcat) manual](https://man.openbsd.org/nc.1)
- [Linux `cat` manual](https://man7.org/linux/man-pages/man1/cat.1.html)

**Security note:** Never publish actual passwords or other credentials in a public repository.