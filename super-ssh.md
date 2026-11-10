# Write-up: Super SSH

| Field | Details |
|---|---|
| **Platform** | SciLab Academy (sciLabAcademy.net) |
| **Category** | Linux / Networking — Remote Access |
| **Difficulty** | Easy |
| **Date completed** | November 2026 |
| **Time spent** | (fill in) |
| **Tools used** | Kali Linux terminal, `ssh` client |

> No flag value is included below, in line with my code of conduct and platform rules.

## 1. Goal

The challenge statement gave a specific host, port, username and password, and asked me to connect over SSH and retrieve the flag. It directly tested whether I could form and run a correct SSH command with non-default options.

## 2. What I knew and what I tried first

- I knew the basic form of an SSH command is `ssh user@host`, which assumes the default SSH port, 22.
- This challenge specified a **non-default port** (27085), so I knew I needed the `-p` flag to tell the client which port to connect on, since without it the client would try port 22 and fail.
- I also expected to be asked to accept a host key fingerprint, since this would be the first time connecting to this particular server from my machine.

## 3. Steps that worked

1. Built the connection command using the exact details given: username `ctfplayer`, host `zebek.scilabacademy.net`, and port `27085`:
   ```
   ssh -p 27085 ctfplayer@zebek.scilabacademy.net
   ```
2. Pressed Enter to run the command.
3. Was prompted to confirm the server's host key fingerprint (standard behavior the first time connecting to a new SSH server, so the client can warn about possible impersonation in future). Typed `yes` to accept and continue.
4. Was prompted for a password and entered the one provided by the challenge (`FD7746`).
5. Successfully logged into the remote shell and located the flag.

## 4. What I found

This was less about finding a "vulnerability" and more about correctly using the SSH protocol with a non-standard port and understanding the host-key verification step, which many beginners skip past without understanding.

## 5. Why it happened (root cause)

Not applicable in the usual vulnerability sense. This was a skills challenge confirming the ability to connect to a remote Linux host securely rather than a security flaw to exploit.

## 6. Impact

Not applicable (no vulnerability). In a real-world sense, understanding SSH properly matters because:
- Non-default ports are often used to reduce automated scanning noise (not a real security control by itself, but common in practice).
- Correctly verifying host key fingerprints is how SSH protects against machine-in-the-middle attacks. Blindly accepting fingerprints without checking them (which I did here, since it was a known training environment) would not be safe practice on an unknown, untrusted real-world server.

## 7. Category and standards

- Maps to foundational **secure remote administration** knowledge (Cisco Module 12: "Telnet, SSH and SCP" from my study notes, and Module 19 on monitored protocols).
- Related security principle: **always verify host keys on real systems**; only accept them blindly in throwaway lab/training environments where you already trust the source.

## 8. How to fix it

Not applicable as a vulnerability fix. As a best-practice note for future real-world use:
- On a real, unknown server, I should verify the host key fingerprint through a separate trusted channel before typing `yes`, rather than accepting it automatically.
- Prefer SSH key-based authentication over passwords where possible, since it is far more resistant to brute-force and credential-stuffing attacks.

## 9. What I learned

- The `-p` flag is required whenever an SSH service runs on a port other than the default 22.
- The host key fingerprint prompt is a real security feature (part of protecting against machine-in-the-middle attacks), not just a formality, and should be taken seriously on real systems even though I accepted it quickly here in a trusted training context.
- Reading a challenge's connection details carefully (user, host, port, password) and mapping each one to the correct SSH flag is itself the core skill being tested.

## 10. What I will practise next

Set up SSH **key-based authentication** (instead of password login) between my Kali VM and a second practice VM in my isolated lab, to learn the more secure, real-world standard method of connecting.
