# Password Strength Checker

Most password checkers just count characters and show a colored bar. I wanted to see what actually happens when someone attacks a password. This app takes a password, hashes it and runs a real brute-force attack on it with [Hashcat](https://hashcat.net/hashcat/). While Hashcat is working, you can watch the progress live in the browser: how fast it guesses, how long it thinks it will take, and whether it has already found the password.

## How it works

1. You enter a password and choose a hash algorithm: MD5, SHA-256 or bcrypt.
2. Optionally, you enter a Hashcat mask to limit the search, for example `?l?l?l?l` for four lowercase letters.
3. The Node.js server hashes the password and starts Hashcat as a brute-force (mask) attack on that hash.
4. The server reads Hashcat's status output every second and sends it to the browser over a WebSocket.
5. The page shows the status, start time, estimated time, current mask, guessing speed and the GPU being used. You can stop the attack at any time.

Comparing the algorithms makes the point quickly: a short password hashed with MD5 is often cracked in seconds, while the same password with bcrypt takes far longer.

## Tech stack

- **Frontend:** HTML, CSS, JavaScript
- **Backend:** Node.js, Express, WebSocket (`ws`)
- **Hashing:** Node `crypto` (MD5, SHA-256), `bcrypt`
- **Cracking:** Hashcat 6.2.3

## Run it locally

You need Node.js and [Hashcat](https://hashcat.net/hashcat/) installed. The app currently runs on **Windows only** and expects Hashcat in `C:\hashcat-6.2.3`. If yours is somewhere else, change `hashcatPath` in `server/index.mjs`.

```bash
git clone https://github.com/RaedNaamoun/password-strength-checker.git
cd password-strength-checker
npm install
npm start
```

Then open http://localhost:3000. The WebSocket server runs on port 8080.
