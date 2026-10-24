# Bandit Level 15 → Level 16

## Challenge Information

- **Platform:** OverTheWire Bandit
- **Category:** Linux, Networking, SSL/TLS
- **Difficulty:** Beginner
- **Tools Used:** `openssl s_client`, SSH
- **Objective:** Submit the current level's password to a service on `localhost:30001` using an SSL/TLS-encrypted connection to retrieve the next password.

## 1. Challenge Description

In this level, the next password must be retrieved by sending the current level's password to a service listening on port `30001`.

Unlike the previous level, which used a plain Netcat connection, this challenge requires an encrypted connection using SSL/TLS.

## 2. Connecting to the TLS Service

After logging in as `bandit15`, I ran:

```bash
openssl s_client -connect localhost:30001
```

### Command breakdown

- `openssl` is a toolkit for cryptographic operations and secure communications.
- `s_client` starts a client that can connect to an SSL/TLS server.
- `-connect localhost:30001` specifies the destination hostname and port.
- `localhost` refers to the same machine on which the command is running.
- `30001` is the port used by the challenge's service.

The command attempted to establish a TLS connection and displayed connection and certificate details.

## 3. Submitting the Password

After the connection was established, I entered the current level's password into the terminal and pressed Enter.

The password was sent through the TLS connection to the server.

The server responded with:

```text
Correct!
```

It then returned the password for `bandit16`. I kept the actual password private and excluded it from this write-up.

## 4. Understanding the TLS Connection

**TLS (Transport Layer Security)** protects data exchanged between a client and a server by providing encryption and integrity protection, and it can authenticate the server using certificates.

In this challenge:

- `openssl s_client` acted as the TLS client.
- The service on port `30001` acted as the server.
- The password was submitted through the encrypted connection.
- The server validated the password and returned the next one.

### What about the self-signed certificate?

A self-signed certificate is signed by its own issuer rather than by a publicly trusted certificate authority. It may trigger a certificate trust warning because the client cannot establish trust through its usual certificate authority chain.

A certificate warning does not necessarily mean the TLS connection failed. However, encryption alone does not guarantee that the server's identity has been verified. In a real environment, certificate warnings should be investigated rather than ignored.

## 5. Key Learnings

- **`openssl s_client`:** Establishes SSL/TLS client connections and displays connection details.
- **TLS:** Protects data exchanged between communicating systems.
- **`localhost`:** Refers to the local machine, typically through the loopback interface.
- **Port `30001`:** Identifies the service endpoint used by this challenge.
- **Digital certificates:** Help establish a server's identity during TLS connections.
- **Self-signed certificate:** A certificate signed by its own issuer, which may not be trusted by default.

## 6. Next Level Login

After retrieving the password, I can attempt to log in as `bandit16`:

```bash
ssh -p 2220 bandit16@bandit.labs.overthewire.org
```

Enter the password obtained from the TLS service when prompted.

## 7. References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [OpenSSL `s_client` documentation](https://docs.openssl.org/3.0/man1/openssl-s_client/)
- [RFC 8446 — The TLS Protocol Version 1.3](https://www.rfc-editor.org/rfc/rfc8446)

**Security note:** Never publish passwords or private credentials in a public repository.