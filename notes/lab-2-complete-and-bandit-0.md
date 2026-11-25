# Lab 2 Complete, and First Steps into OverTheWire Bandit

**Week:** 2 to 3
**Date:** November 2026
**Status:** Lab 2 done; Bandit level 0 solved, level 1 login pending

## Lab 2: Identity commands

Finished the last section of Lab 2 — understanding user identity and admin rights.

```bash
whoami
id
sudo whoami
```

| Command | What it shows |
|---|---|
| `whoami` | Just the current username |
| `id` | Fuller picture: user ID number, primary group, and **every** group I belong to |
| `sudo whoami` | Runs `whoami` with root's identity for just that one command |

**Key detail from `id`:** the output lists `groups=1000(kali),27(sudo),...`. Being a member of the `sudo` group is literally what grants permission to use `sudo` at all — it isn't automatic for every user.

**Key insight from `sudo whoami`:** running it prints `root`, but I'm still logged in as `kali` the whole time — nothing permanently changes. `sudo` grants temporary, command-by-command elevated access rather than a permanent login switch, which is safer since it prevents accidentally running a destructive command while permanently "being" root.

## Starting OverTheWire Bandit

Set up the connection pattern for Bandit, which uses SSH on a non-default port, same structure as the earlier Super SSH challenge:

```bash
ssh -p 2220 bandit0@bandit.labs.overthewire.org
```

**Note:** flags like `-p 2220` can move around in the command since they're named, not positional — `ssh -p 2220 user@host` and `ssh user@host -p 2220` both work identically. What must stay fixed is `user@host` as the final piece.

**Important early mistake caught:** initially ran the command without the `bandit0@` username prefix. Without an explicit username, SSH defaults to trying the *local* Kali username instead, which gets rejected by the remote server — producing a "Permission denied" that looks exactly like a wrong password, but is actually a missing username.

### Level 0: finding the password

The instructions said the password for the next level is stored in a file called `readme` in the home directory. Solved with:

```bash
cat readme
```

This printed the password needed to log into `bandit1`.

### Troubleshooting: the localhost redirect

Attempting to connect to `bandit1` produced this error:

```
!!! You are trying to log into this SSH server from localhost.
!!! Connecting from/to localhost is blocked to conserve resources.
```

**Diagnosis process:**
1. Noticed the host key prompt showed `[127.0.0.1]:2220` instead of a real external IP — `127.0.0.1` always means "this same machine."
2. Checked `/etc/hosts` and found a manually added line:
   ```
   127.0.0.1 bandit bandit.labs.overthewire.org
   ```
   This file overrides normal DNS lookups, and it was redirecting several OverTheWire game domains (Bandit, Behemoth, Narnia, and more) straight back to my own Kali VM instead of OverTheWire's real server.
3. Confirmed with `nslookup bandit.labs.overthewire.org`, which showed the hijacked result pointing to `localhost`.

**Fix identified (to apply next session):**
```bash
sudo nano /etc/hosts
```
Delete the line `127.0.0.1 bandit bandit.labs.overthewire.org`, save with Ctrl+O, Enter, then exit with Ctrl+X. Then retry the SSH login to `bandit1`.

## Why this troubleshooting matters

This is effectively a miniature, self-inflicted version of a real attack technique: **hosts-file hijacking**, where a trusted domain name is secretly redirected to a different address. Diagnosing it the same way a defender would — checking the resolved IP, inspecting `/etc/hosts`, confirming with `nslookup` — was good practice for real troubleshooting and investigation habits.

## Next step

Apply the `/etc/hosts` fix, then log into `bandit1` using the password found in level 0, and continue the Bandit wargame from there.
