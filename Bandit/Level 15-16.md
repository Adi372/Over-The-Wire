# Bandit Level 15 → Level 16

## Level Goal
Retrieve the next level's password by sending the current level's password to `localhost` on port `30001` using SSL/TLS encryption.

## Commands Used
```bash
openssl s_client -connect localhost:30001
```

Then entered the current level's password into the connected session and pressed Enter.

## Steps
1. Logged in as `bandit15` using SSH.
2. Used `openssl s_client -connect localhost:30001` to establish a TLS connection to port `30001`.
3. Waited for the connection to be established.
4. Entered the current level's password and pressed Enter.
5. The server responded with `Correct!` and returned the next level's password.

## Key Learning
- `openssl s_client`: Connects to a server using SSL/TLS and displays connection details.
- **TLS:** Encrypts data sent between a client and a server.
- `localhost`: Refers to the same machine.
- **Port `30001`:** The network endpoint used by this level's service.
- **Self-signed certificate:** A certificate not signed by a publicly trusted certificate authority. The warning here did not prevent the lab connection from working.

## Next Level Login
```bash
ssh -p 2220 bandit16@bandit.labs.overthewire.org
```

**Note:** Save the password privately. Do not include it in the README.
