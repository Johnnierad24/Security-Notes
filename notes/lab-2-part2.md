# Lab 2, Part 2: Locking Down a File and Making a Script Executable

**Week:** 2
**Date:** November 2026
**Status:** In progress

## What I did

Continued Lab 2 with two sections: locking a file down to private, and creating and running a Bash script.

### Section: Lock down a private file

```bash
chmod 600 secret.txt
ls -l secret.txt
```

**Before:** `-rw-r--r--` (644) — owner can read/write, group and others can read
**After:** `-rw-------` (600) — owner can read/write, group and others get nothing at all

| | Before (644) | After (600) |
|---|---|---|
| Owner | `rw-` | `rw-` (unchanged) |
| Group | `r--` | `---` |
| Others | `r--` | `---` |

Owner access never changed — only group and others lost read access. This is why 600 is the standard number for private files: it locks out literally everyone except the owner.

### Section: Make a script and run it

```bash
echo 'echo Hello from my script' > hello.sh
./hello.sh
```

The single quotes around `echo Hello from my script` let the word `echo` be written **into the file's content** without it running immediately. This created `hello.sh` containing one line of valid Bash — but just as plain text, not yet executed.

Running `./hello.sh` (the `./` tells Linux to look in the current folder) failed with:

```
bash: ./hello.sh: Permission denied
```

**Why:** new files default to 644 (`rw-r--r--`), which has no execute permission. Read/write access and permission to *run* a file are completely different things.

**Fix:**

```bash
chmod +x hello.sh
./hello.sh
```

`+x` (symbolic mode) added execute permission for owner, group and others at once, without touching read/write. Running it again printed:

```
Hello from my script
```

## Key takeaway

A `.sh` file is just a text file — nothing about the extension makes it runnable. What actually allows execution is purely the **execute permission bit**, toggled on with `chmod +x`.

## Next step

Continue Lab 2 with the remaining sections: checking `whoami`/`id`/`sudo whoami` to see user identity and admin rights.
