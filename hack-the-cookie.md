# Write-up: Hack the Cookie

| Field | Details |
|---|---|
| **Platform** | HackerDNA |
| **Category** | Web — Session Management / Cookie Poisoning |
| **Difficulty** | Easy |
| **Date completed** | November 2026 |
| **Time spent** | (fill in) |
| **Tools used** | Browser DevTools (Application tab), CyberChef |

> No flag value is included below, in line with my code of conduct and platform rules.

## 1. Goal

The challenge gives a normal user session and asks you to escalate your privileges to an admin or otherwise privileged role by manipulating the data the server trusts about who you are.

## 2. What I knew and what I tried first

- Websites use cookies to remember who is logged in between requests, since HTTP itself does not keep you "logged in" by default.
- I suspected the cookie might hold more than just a session ID, since the challenge name points directly at the cookie.
- My first step was simply to look at what the cookie actually contained, rather than guessing.

## 3. Steps that worked

1. Opened the browser's **DevTools** (F12) and went to the **Application** tab (in Chrome; it's called **Storage** in Firefox).
2. Found the **Cookies** section for the site and selected the relevant cookie.
3. Copied its value, which was a long string of mixed letters and numbers.
4. Pasted the value into **CyberChef** and applied a **"From Base64"** recipe.
5. The decoded output was a **JSON object** in plain text (not a hash, not hex) containing fields describing the session, including a role or username field.
6. Edited the decoded JSON directly in CyberChef's output, changing the role/username field to an admin-style value.
7. Reversed the process: cleared the existing recipe and applied a **"To Base64"** recipe to the edited JSON, producing a new encoded string.
8. Replaced the original cookie's value in DevTools with this new encoded string.
9. Refreshed the page. The server read the cookie, trusted the role it claimed, and granted access as that privileged role.

## 4. What I found

The application stores **authorization data** (the user's role) client-side, inside a cookie that is only Base64-**encoded**, not signed or encrypted. Base64 is an encoding, not a security control. Anyone can decode it, change it, and re-encode it.

## 5. Why it happened (root cause)

The server trusted a value supplied by the client without any way to verify it had not been tampered with. There was no:

- **Server-side session store** (the authoritative copy of "who is this user and what is their role" kept on the server, not the browser)
- **Signature or integrity check** (such as a signed token) to detect that the cookie had been modified
- **Re-validation** of the user's actual permissions against a trusted source on each request

## 6. Impact

In a real application, this would let any user grant themselves administrator access, bypassing the entire authorization system. Depending on what the admin role can do, this could mean reading or modifying other users' data, changing settings, or taking over the application entirely.

## 7. Category and standards

- **OWASP Top 10 (2021): A01 – Broken Access Control**
- **OWASP Top 10: A02 – Cryptographic Failures** (encoding mistaken for protection)
- Related concept from my notes: never trust client-supplied data for authorization decisions; always check on the server.

## 8. How to fix it

- Keep the session and role information **server-side** (a session store or database), and give the client only an opaque, random **session ID**.
- If role data must travel with the client, use a **signed token** (such as a properly implemented JWT) so any tampering invalidates the signature, and **never trust unsigned claims**.
- Re-check the user's actual role from the server's own records on every privileged action, not just from what the cookie claims.
- Treat Base64 as a **data format**, never as a security measure. It protects nothing by itself.

## 9. What I learned

- Encoding (Base64) is reversible and provides no security; it is not the same as encryption or a signature.
- Client-side storage, including cookies, is fully visible and editable by the user, so it should never be the single source of truth for authorization.
- Reading tool output carefully (plain text JSON vs. hash vs. hex) tells you what kind of decoding step to try next.

## 10. What I will practise next

Try a similar challenge involving **JWTs** specifically, to see how signed tokens behave differently from a plain encoded cookie, and practise spotting when a JWT's signature is not actually being verified by the server.
