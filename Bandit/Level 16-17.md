# Bandit Level 16 → Level 17

## Challenge Information

- **Platform:** OverTheWire Bandit
- **Category:** Linux, Network Scanning, SSL/TLS, SSH Authentication
- **Difficulty:** Beginner
- **Tools Used:** `nmap`, `openssl s_client`, `chmod`, `ssh`
- **Objective:** Find the correct service listening on a localhost port between `31000` and `32000` that supports SSL/TLS and provides the credentials for the next level.

## 1. Challenge Description

This level requires identifying the correct service among several ports on the local machine.

The process involves scanning the port range, testing the open ports for SSL/TLS support, and identifying the service that returns the credentials required to access `bandit17`.

## 2. Scanning the Port Range

I used Nmap to discover open ports between `31000` and `32000`.

```bash
nmap -p 31000-32000 localhost
```

### Command breakdown

- `nmap` scans hosts and network services.
- `-p 31000-32000` specifies the port range to scan.
- `localhost` targets the machine on which I am currently logged in.

The scan identified several open ports that needed further investigation.

**Why this step matters:** An open port indicates that a service is accepting connections, but it does not automatically tell us whether that service supports TLS or performs the task required by the challenge.

## 3. Testing the Open Ports

I tested the discovered ports using OpenSSL:

```bash
openssl s_client -connect localhost:31046
openssl s_client -connect localhost:31518
openssl s_client -connect localhost:31691
openssl s_client -connect localhost:31790
openssl s_client -connect localhost:31960
```

The `openssl s_client` command attempts to establish an SSL/TLS connection with the specified service.

Some connections produced errors or did not return the required credentials. This helped narrow down the possible candidates.

### Understanding the command

- `openssl` provides cryptographic and secure communication utilities.
- `s_client` starts an SSL/TLS client.
- `-connect localhost:31790` specifies the target host and port.

Testing a port with OpenSSL helps determine whether it can participate in a TLS handshake, but a successful TLS connection alone does not prove that it is the correct service.

## 4. Identifying the Correct Service

After testing the candidate ports, I identified port `31790` as the service that returned the required credentials.

I used the following command to reduce extra connection output:

```bash
openssl s_client -quiet -connect localhost:31790
```

The `-quiet` option suppresses some diagnostic output, making the application-level response easier to read.

The service returned the credentials for the next level, including a private SSH key. I saved the private key locally as `bandit17.key`.

**Security note:** The actual credentials and private key are not included in this write-up.

## 5. Fixing Private Key Permissions

My first SSH connection attempt failed because the private key's permissions were too open.

SSH checks private key permissions to prevent other users from accessing sensitive authentication material. I corrected the permissions with:

```bash
chmod 600 bandit17.key
```

This gives the file owner read and write permissions while removing permissions for the group and other users.

## 6. Connecting as bandit17

With the permissions corrected, I used the private key to authenticate:

```bash
ssh -i bandit17.key bandit17@bandit.labs.overthewire.org -p 2220
```

### Command breakdown

- `ssh` starts a secure remote connection.
- `-i bandit17.key` specifies the private key used for authentication.
- `bandit17@bandit.labs.overthewire.org` specifies the target account and server.
- `-p 2220` specifies the SSH port.

The connection succeeded, completing the level.

## 7. Key Learnings

- **Nmap:** Discovers open ports and helps identify services running on a host.
- **Port scanning:** Checks a specified range of network ports for accessible services.
- **`localhost`:** Refers to the local machine, typically through the loopback interface.
- **`openssl s_client`:** Tests SSL/TLS connections and displays connection details.
- **TLS handshake:** Establishes the parameters needed to create a secure connection.
- **`-quiet`:** Reduces some OpenSSL diagnostic output.
- **`chmod 600`:** Restricts private key file access to its owner.
- **`ssh -i`:** Uses a specified private key for SSH authentication.

## 8. Next Level

The private key obtained from the service can be used to access `bandit17`:

```bash
ssh -i bandit17.key bandit17@bandit.labs.overthewire.org -p 2220
```

The next challenge involves comparing files to find the password for the following level.

## 9. References

- [OverTheWire Bandit](https://overthewire.org/wargames/bandit/)
- [Nmap Reference Guide](https://nmap.org/book/man.html)
- [OpenSSL `s_client` documentation](https://docs.openssl.org/3.0/man1/openssl-s_client/)
- [`chmod` manual](https://man7.org/linux/man-pages/man1/chmod.1.html)
- [`ssh` manual](https://man7.org/linux/man-pages/man1/ssh.1.html)

**Security note:** Never commit passwords or private SSH keys to a public repository. Keep credentials stored locally with appropriate permissions.