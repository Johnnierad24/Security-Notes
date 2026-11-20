# Lab 2, Part 1: Making a Private File and Checking Permissions

**Week:** 2
**Date:** November 2026
**Status:** In progress

## What I did

Also did extra practice on Lab 1 first: creating/verifying files, copying and moving files into folders with `cp`/`mv` using a folder as the destination (instead of just renaming), and listing a folder's contents without `cd`-ing into it (`ls path/to/folder`).

Then started Lab 2 with the first section: creating a file and inspecting its default permissions.

```bash
cd ~/lab
echo "top secret" > secret.txt
ls -l secret.txt
```

## What each command does

| Command | What it does |
|---|---|
| `cd ~/lab` | Moves back into the main lab folder |
| `echo "top secret" > secret.txt` | Creates `secret.txt` and writes the text into it in one step (no separate `touch` needed when using `>` on a new file) |
| `ls -l secret.txt` | Shows the long-format details for one specific file, instead of the whole folder |

## What I saw

```
-rw-r--r-- 1 kali kali 11 Nov 20 14:32 secret.txt
```

## Reading the output

- `-rw-r--r--` → owner `rw-`, group `r--`, others `r--` → permission **644**, the default for a newly created file
- `kali kali` → the owner and the group (both `kali`, my user)
- `secret.txt` → the filename

## Why this matters

This is the "before" state: a brand-new file is created with fairly open default permissions (644 — readable by everyone on the system, writable only by the owner). The next step in Lab 2 is to lock this down to `600` so that only the owner can read or write it at all, using `chmod 600 secret.txt`.

## Next step

Continue Lab 2: apply `chmod 600 secret.txt`, re-check with `ls -l`, and confirm the permission change took effect.
