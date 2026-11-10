# Write-up: Secrets in Source 2 (Bypass Security Through Obscurity)

| Field | Details |
|---|---|
| **Platform** | HackerDNA |
| **Category** | Web — Security Through Obscurity / Information Disclosure |
| **Difficulty** | Easy |
| **Date completed** | November 2026 |
| **Time spent** | (fill in) |
| **Tools used** | Browser `view-source:` |

> No flag value or exact hidden path is included below, in line with my code of conduct and platform rules.

## 1. Goal

A follow-up to the first "Secrets in Source" challenge, again with right-click and F12 blocked, but this time the URL itself had an extra part (a URL fragment) that needed to be handled correctly to get a clean view of the page source.

## 2. What I knew and what I tried first

- I already knew from the previous challenge that `view-source:` placed in front of a URL bypasses page-level blocks on right-click and DevTools.
- This page's URL included a **fragment** at the end, written after a `#` symbol (for example `.../145-secrets-in-source-2/#services`). I knew that the part after `#` is handled entirely by the browser on the client side and is mainly used to jump to a section of the page or trigger client-side navigation, rather than being sent to the server as part of the request.

## 3. Steps that worked

1. Took the full URL, including the fragment after the `#`.
2. Removed the fragment portion (everything from `#` onward), keeping only the base page URL.
3. Placed `view-source:` directly in front of that cleaned-up URL.
4. Loaded it, which displayed the raw HTML of the actual page rather than whatever the `#` fragment would have triggered client-side.
5. Found a hint inside an HTML comment pointing to a hidden path, similar in structure to the first challenge.
6. Appended that hidden path to the site's base URL and navigated to it directly to retrieve the flag.

## 4. What I found

The same underlying issue as "Secrets in Source" 1: a hidden path revealed in a source-code comment, with no real access control protecting it. The extra step here was understanding that the **URL fragment had to be excluded** for `view-source:` to render the actual page HTML cleanly instead of being confused by the fragment.

## 5. Why it happened (root cause)

- The core issue is the same as before: **security through obscurity**, meaning the developer assumed that if something is hard to find (a random-looking path, or a page behind a fragment-based "navigation"), it does not need real protection.
- The URL fragment detail is a client-side quirk, not a server-side security measure. It just means "fragments are not security boundaries," since the server never even sees them.

## 6. Impact

Same class of impact as the related challenge: confidential files or paths can be discovered simply by reading what the application reveals about itself, without any real exploitation technique, because there is no genuine access control layer behind the obscurity.

## 7. Category and standards

- **OWASP Top 10 (2021): A05 – Security Misconfiguration**
- Specifically demonstrates why **"security through obscurity" is not a valid security control** on its own. Obscurity can be one *minor* layer, but it must never be the only layer.

## 8. How to fix it

- Apply the same fixes as Secrets in Source 1: strip sensitive comments from production code, and put real authentication/authorization checks on any path that should not be public.
- Do not assume URL structure (fragments, random folder names, "hidden" pages) provides any actual security, since fragments are visible and editable entirely client-side and random names can be found exactly as I found them.
- If something must not be public, it should return a proper "forbidden" or "not found" response to unauthorized requests, regardless of whether the exact path is known.

## 9. What I learned

- The part of a URL after `#` (the fragment) is **never sent to the server**; it is purely a browser/client-side mechanism. This matters both for understanding web behavior and for cleanly using tools like `view-source:`.
- "Security through obscurity" is a recognized **anti-pattern**: relying on secrecy of implementation instead of actual controls. It can slow down a casual attacker but does nothing against anyone who looks carefully, as this challenge demonstrated.
- Small URL details (like a trailing fragment) can change how a technique needs to be applied, so reading the full URL carefully matters.

## 10. What I will practise next

Explore how **URL fragments** are used in real single-page applications (SPAs) for client-side routing, to better understand the difference between what the server sees and what only the browser handles.
