# Write-up: Hack the Login

| Field | Details |
|---|---|
| **Platform** | HackerDNA |
| **Category** | Web — Information Disclosure / Source Code Review |
| **Difficulty** | Easy |
| **Date completed** | November 2026 |
| **Time spent** | (fill in) |
| **Tools used** | Browser "View Page Source", DevTools (Sources tab) |

> No flag value is included below, in line with my code of conduct and platform rules.

## 1. Goal

The challenge presents a login page. The task is to gain access or find the flag without being given valid credentials up front.

## 2. What I knew and what I tried first

- Login pages often rely on client-side JavaScript to handle at least part of the logic (showing errors, sometimes even validating input), so checking what code the browser actually loaded was a reasonable first move.
- Before touching the login form itself, I wanted to see exactly what files the page pulled in.

## 3. Steps that worked

1. Right-clicked the page and chose **View Page Source** to read the raw HTML as delivered by the server.
2. Looked through the `<script>` tags to identify the name of a JavaScript file the page loaded.
3. Opened **DevTools** (F12) and went to the **Sources** tab.
4. Located that same script file in the file tree and read through its contents.
5. Found the flag sitting directly inside that client-side script file.

## 4. What I found

Sensitive data (in this case, the flag, but in a real app this would typically be a credential check, an API key, or hard-coded logic) was placed inside **client-side JavaScript**, which is fully downloaded and readable by anyone who visits the page.

## 5. Why it happened (root cause)

The developer treated the browser as a trusted, private place to put logic or data, when in fact **anything sent to the client can be read by the client**. There was no separation between what the server should keep secret and what the browser is allowed to see.

## 6. Impact

In a real login system, this kind of mistake could expose:
- Hard-coded credentials or API keys
- The actual validation logic (letting an attacker see exactly what the check expects and craft an input that passes it)
- Internal application details useful for further attacks

This directly undermines the authentication system, since an attacker does not need to guess anything, they just read the source.

## 7. Category and standards

- **OWASP Top 10 (2021): A05 – Security Misconfiguration** (shipping sensitive logic/data to the client)
- Related principle from my notes: **never trust the client**, and never put secrets where the client can read them.

## 8. How to fix it

- Keep all authentication logic and secret values **on the server**. The client should only send credentials and receive a success/failure result.
- Audit JavaScript files before deployment for hard-coded secrets, test flags, or debug logic.
- Use build tools that strip comments, debug code, and unused logic from production bundles.
- Treat "it's just in the frontend code" as equivalent to "it's public."

## 9. What I learned

- `View Page Source` and the DevTools **Sources** tab are often the very first places to check in any web challenge, before trying to "hack" anything actively.
- Client-side JavaScript is not a hiding place. If the browser can run it, the browser (and therefore the user) can read it.
- Simple recon (reading what's already there) solves more challenges than people expect.

## 10. What I will practise next

Look for a challenge where the hidden logic is slightly obfuscated or minified, to practise reading and understanding compressed JavaScript rather than clean code.
