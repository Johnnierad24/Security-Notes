# Daily Log: Lab 1, Part 1

**Week:** 2
**Date:** November 2026

## What I did today

Started Lab 1 (Linux navigation and files) hands-on in Kali. Completed the first section: creating a practice area and moving into it.

```bash
mkdir -p ~/lab/week1
cd ~/lab/week1
touch notes.txt
echo "my first note" > notes.txt
cat notes.txt
```

## Commands I practised and what they do

| Command | What it does |
|---|---|
| `mkdir -p` | Creates a folder, and any missing parent folders along the way, without erroring if they already exist |
| `cd` | Moves into a folder |
| `touch` | Creates a new empty file |
| `echo "text" > file` | Writes text into a file, overwriting it |
| `cat` | Prints a file's contents, used here to confirm the write worked |

## Why one command at a time

Ran each line individually and pressed Enter, checking the result before moving to the next, rather than pasting the whole block at once. This way I see what each command actually changed and catch mistakes immediately instead of several steps deep.

## Status

Lab 1 — in progress. Remaining sections (copy/rename/delete, search and count, `man` pages) to continue next session.

## Goal

Know these core commands "by fingertips," since Linux fundamentals (Weeks 2 to 3) are the foundation everything else in the 24-week plan builds on.
