
# Web Authentication Brute-Force: Custom Scripting

## Overview
This project demonstrates an automated dictionary attack against a web application's login portal. When standard automated tools (like Hydra) produced False Positives due to parsing bugs with session cookies, a custom Bash script was developed to reliably execute the brute-force attack and validate the credentials.

## Environment & Tools
* **Target Environment:** DVWA (Damn Vulnerable Web Application) hosted on Windows 11.
* **Attacker Machine:** Kali Linux.
* **Core Tools Used:** Bash, `curl`, `grep`.
* **Technique:** HTTP GET Brute-Force with Session Hijacking.

## Execution Steps

### 1. Dictionary Creation
A custom wordlist (`parole.txt`) was generated containing common weak passwords to optimize the attack simulation.

### 2. Session Hijacking
To bypass initial access controls, an active session cookie (`PHPSESSID`) was extracted from the target application using Developer Tools. This cookie was necessary to authenticate the automated HTTP requests.

### 3. Custom Bash Exploit
A custom one-line Bash script was crafted to loop through the wordlist. It used `curl` to send GET requests with the injected session cookie and `grep` to parse the server's HTML response. The script was designed to silently discard incorrect attempts and explicitly highlight the successful payload.

### 4. Proof of execution
![Hydra brute force](./brute_force)
**The Script:**
```bash
for pass in $(cat parole.txt); do resultat=$(curl -s -b "PHPSESSID=[COOKIE]; security=low" "http://[IP]/dvwa/vulnerabilities/brute/?username=admin&password=$pass&Login=Login"); if echo "$rezultat" | grep -q "incorrect"; then echo "[-] $pass -> incorrect"; else echo -e "\e[32m[+] Correct password is: $pass\e[0m"; fi; done

