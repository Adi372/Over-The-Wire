# Bandit Level 14 → Level 15

## Level Goal
Retrieve the next level's password by submitting the current level's password to port `30000` on `localhost`.

## Commands Used
```bash
cat /etc/bandit_pass/bandit14
cat /etc/bandit_pass/bandit14 | nc localhost 30000
```

## Steps
1. Used `cat` to read the current level's password from `/etc/bandit_pass/bandit14`.
2. Piped the password into `nc localhost 30000`.
3. `nc` (Netcat) connected to port `30000` on the local machine and sent the password.
4. The server responded with `Correct!` and returned the next level's password.

## Key Learning
- `localhost`: Refers to the same machine, usually through the loopback address `127.0.0.1`.
- **Port:** Identifies a specific service or application endpoint on a machine.
- `nc` (Netcat): Connects to network ports and sends or receives data.
- `|` (pipe): Passes the output of one command as input to another command.
- **Port 30000:** The service listens on this port and checks the submitted password.

## Next Level Login
```bash
ssh -p 2220 bandit15@bandit.labs.overthewire.org
```

**Note:** Save the password privately. Do not include it in the README.
