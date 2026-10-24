# Bandit Level 20 → Level 21

## Challenge Information

- **Platform:** OverTheWire Bandit
- **Category:** Linux, Networking, Setuid, Client-Server Communication
- **Difficulty:** Beginner
- **Tools Used:** `nc` (Netcat), `./suconnect`, `cat`, SSH
- **Objective:** Use the setuid binary `suconnect` to connect to a local port, submit the current level's password, and receive the password for the next level.

## 1. Challenge Description

The home directory contains a setuid executable named `suconnect`. It connects to a port on `localhost`, reads a line of text from that connection, and compares it with the password for `bandit20`.

If the submitted password matches, the program sends back the password for `bandit21`.

To solve this level, I needed to start a local network listener, connect to it using `suconnect`, and send the correct password through the connection.

## 2. Starting a Netcat Listener

I opened a terminal as `bandit20` and started a Netcat listener on port `1234`:

```bash
nc -l -p 1234
```

### Command breakdown

- `nc` starts Netcat, a tool for network communication.
- `-l` puts Netcat into listening mode.
- `-p 1234` specifies port `1234`.

The listener waits for an incoming connection. Once connected, text sent through the connection can be received by the listener.

## 3. Connecting with the Setuid Binary

In a second terminal, I ran:

```bash
./suconnect 1234
```

The program connected to port `1234` on `localhost`.

The first terminal displayed the line sent by `suconnect`:

```text
4pIjcunZ0fK2Vmp3IwfG8V7VhXD6p0A
```

This was the current level's password being sent through the connection. The actual password is omitted from this write-up.

## 4. Submitting the Password

I copied the password displayed in the listener terminal and entered it into the Netcat session, then pressed Enter.

The `suconnect` program received the input and checked it against the password for `bandit20`.

The second terminal displayed:

```text
Read: [submitted password]
Password matches, sending next password
```

This confirmed that the password was correct. The program then sent the password for `bandit21` through the connection, which appeared in the Netcat terminal.

I kept the next-level password private and did not include it in this write-up.

## 5. Understanding the Two-Terminal Setup

The two terminals served different purposes:

- **Terminal 1:** Ran the Netcat listener and received data from `suconnect`.
- **Terminal 2:** Ran `./suconnect 1234`, which connected to the listener and validated the password received from it.

The connection worked because both programs communicated through port `1234` on the same machine.

This demonstrates a basic client-server communication pattern: one program listens for connections, while another connects and exchanges data.

## 6. Key Learnings

- **Netcat (`nc`):** Creates network connections and can listen for incoming connections.
- **Listening mode (`-l`):** Waits for another program to connect.
- **Localhost:** Refers to the same machine, typically through the loopback interface.
- **Port:** Identifies a communication endpoint used by a network service.
- **Setuid:** Allows an executable to run with the effective user ID of its owner, subject to system rules.
- **`suconnect`:** Connects to a specified local port and checks the received password.
- **Two-terminal workflow:** Helps test client-server communication when one process must listen while another connects.
- **Password validation:** The program returns the next password only after the submitted password matches the current level's password.

## 7. Next Level Login

After retrieving the password, connect as `bandit21`:

```bash
ssh -p 2220 bandit21@bandit.labs.overthewire.org
```

Enter the password returned by `suconnect` when prompted.

## 8. References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [Netcat manual](https://man.openbsd.org/nc.1)
- [`ssh` manual](https://man7.org/linux/man-pages/man1/ssh.1.html)

**Security note:** Never publish passwords or private credentials in a public repository. Use setuid executables only on systems where you have permission to test them.