# Bandit Level 0

## Challenge Information

- **Platform:** OverTheWire Bandit
- **Category:** Linux / SSH
- **Difficulty:** Beginner
- **Tools Used:** SSH, Linux Terminal
- **Objective:** Connect to the OverTheWire Bandit server using SSH.

## 1. Challenge Description

The objective is to establish an SSH connection to the OverTheWire Bandit server using the `bandit0` username and the specified port.

## 2. Initial Connection

Run the following command in the terminal to connect to the server.

```bash
ssh -p 2220 bandit0@bandit.labs.overthewire.org
```

## 3. Authentication

Follow these steps to establish the connection:

1. Open the terminal.
2. Execute the SSH command.
3. Type `yes` if prompted to verify the server's authenticity, after confirming the host key when possible.
4. Enter the Level 0 password provided by OverTheWire.
5. Wait for authentication to complete.

**Key Finding:** A successful login provides access to the remote system as the `bandit0` user.

## 4. Result

Successfully connected to the OverTheWire Bandit server as `bandit0`.

## 5. What I Learned

- **SSH (Secure Shell):** A protocol used to securely access and manage remote systems.
- **Port `2220`:** The port used by the OverTheWire Bandit SSH server.
- **Host Key:** Helps verify the identity of the remote server and detect potential impersonation.
- **Authentication:** Access to the remote system requires valid login credentials.

## 6. References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [OpenSSH Manual](https://man.openbsd.org/ssh)