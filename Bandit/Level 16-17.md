# Bandit Level 16 → Level 17

## Level Goal
Find the correct server listening on a localhost port between `31000` and `32000`. The server must support SSL/TLS and return the credentials for the next level.

## Commands Used

```bash
nmap -p 31000-32000 localhost
openssl s_client -connect localhost:31046
openssl s_client -connect localhost:31518
openssl s_client -connect localhost:31691
openssl s_client -connect localhost:31790
openssl s_client -connect localhost:31960
openssl s_client -quiet -connect localhost:31790
chmod 600 bandit17.key
ssh -i bandit17.key bandit17@bandit.labs.overthewire.org -p 2220
```

## Steps

1. Used `nmap -p 31000-32000 localhost` to find open ports.
2. Tested the open ports with `openssl s_client` to identify servers that support SSL/TLS.
3. Some ports returned errors because they did not communicate properly using TLS.
4. Found the correct TLS server on port `31790`.
5. Used `-quiet` to reduce TLS output and reveal the next-level credentials and private SSH key.
6. Saved the private key as `bandit17.key`.
7. The first SSH attempt failed because the key permissions were too open.
8. Fixed the permissions using `chmod 600 bandit17.key`.
9. Connected successfully as `bandit17`.

## Key Learning

- `nmap -p` scans a specified port range.
- `localhost` refers to the current machine.
- `openssl s_client` tests an SSL/TLS connection.
- `-quiet` reduces extra connection output.
- `chmod 600` allows only the file owner to read and write the private key.
- `ssh -i` specifies which private key to use for authentication.

## Next Level Login

```bash
ssh -i bandit17.key bandit17@bandit.labs.overthewire.org -p 2220
```

## Note

Keep passwords and private SSH keys out of public repositories. Store the key locally and protect it with appropriate permissions.