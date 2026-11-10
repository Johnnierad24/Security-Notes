# Write-up: Secrets in Source

| Field | Details |
|---|---|
| **Platform** | HackerDNA |
| **Category** | Web — Information Disclosure / Security Misconfiguration |
| **Difficulty** | Easy |
| **Date completed** | November 2026 |
| **Time spent** | (fill in) |
| **Tools used** | Browser `view-source:` |

> No flag value or exact hidden path is included below, in line with my code of conduct and platform rules.

## 1. Goal

Find the flag on a page that actively blocks the normal ways of inspecting it (right-click and DevTools are disabled).

## 2. What I knew and what I tried first

- I first tried right-clicking to get "View Page Source" from the context menu, which was blocked.
- I then tried pressing F12 to open DevTools directly, which was also blocked.
- Since both common shortcuts were disabled, I needed another way to see the page's raw HTML that does not depend on the page's own JavaScript being able to intercept it.

## 3. Steps that worked

1. Typed `view-source:` directly in front of the page's URL in the address bar (for example `view-source:https://example.com/page`). This works because `view-source:` is a **browser-level** feature, not something the page's JavaScript can block, since the restrictions on right-click and F12 are enforced by the page's own script, not the browser itself.
2. Read through the raw HTML that was returned.
3. Found an **HTML comment** left in the code by a developer, warning that a specific file/path was still publicly ("world") readable and needed to be locked down before launch.
4. Took the path named in that comment and appended it to the site's base URL.
5. Navigated to that full URL directly, which served the flag file.

## 4. What I found

A **developer comment** left in production HTML revealed the existence and exact location of a file that was never meant to be publicly accessible, and that file had no actual access control protecting it.

## 5. Why it happened (root cause)

Two separate mistakes combined:
- **Leftover debug/internal notes** were shipped to production instead of being stripped out before launch.
- The referenced file had **no server-side access control**, so simply knowing (or guessing) the path was enough to retrieve it. The developer apparently believed the random-looking folder name was protection enough.

## 6. Impact

In a real system, this kind of leak could expose configuration files, credentials, internal documentation, or other sensitive files, just by reading comments left in public-facing code, with no exploitation needed beyond requesting a URL.

## 7. Category and standards

- **OWASP Top 10 (2021): A05 – Security Misconfiguration**
- Closely related to the concept of **security through obscurity** (covered more directly in Secrets in Source 2): hiding something is not the same as securing it.

## 8. How to fix it

- Strip all comments, TODOs, and internal notes from code before it reaches production.
- Never rely on an unguessable-looking path as the only protection for a sensitive file. Use real authentication and authorization checks.
- Run automated code review or linting that flags comments containing words like "secret," "TODO," "fix before launch," etc.
- Regularly audit what static files are actually publicly reachable on a web server.

## 9. What I learned

- `view-source:` is a browser-native way to bypass page-level right-click/F12 blocking, since those blocks rely on JavaScript the page controls, and `view-source:` does not run that JavaScript at all.
- Comments meant for internal developer eyes often end up visible to everyone, so they should never contain sensitive information.
- "Hidden" is not "secured." A path that is hard to guess is still fully accessible to anyone who finds it.

## 10. What I will practise next

Practise identifying other browser-level techniques (like disabling JavaScript entirely, or using browser developer tools accessed via menus instead of F12) for pages that try to block inspection.
