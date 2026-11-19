# Lab 1 Complete: Linux Navigation and Files

**Week:** 2
**Status:** Done
**Date:** November 2026

## Goal

Become comfortable navigating the Linux terminal: creating, viewing, copying, renaming, deleting, and searching files.

## Section 1: Creating a practice area and moving into it

```bash
mkdir -p ~/lab/week1
cd ~/lab/week1
touch notes.txt
echo "my first note" > notes.txt
cat notes.txt
```

| Command | What it does |
|---|---|
| `mkdir -p` | Creates a folder, plus any missing parent folders, without erroring if they already exist |
| `cd` | Moves into a folder |
| `touch` | Creates a new empty file |
| `echo "text" > file` | Writes text into a file, overwriting its contents |
| `cat` | Prints a file's contents, used to confirm the write worked |

## Section 2: Copy, rename and delete

```bash
cp notes.txt backup.txt
mv backup.txt old_notes.txt
ls
rm old_notes.txt
```

| Command | What it does |
|---|---|
| `cp source dest` | Copies a file, leaving both the original and the copy in place |
| `mv old new` | Renames (or moves) a file — unlike `cp`, only one copy exists afterward |
| `ls` | Lists the current folder's contents — used as a checkpoint to confirm what changed |
| `rm` | Permanently deletes a file. No Recycle Bin — always double-check the filename first |

**Key distinction:** `cp` keeps both copies, `mv` keeps only one (relocated/renamed), `rm` keeps none.

## Section 3: Search for text and count lines

Searched log/text content using `grep` for pattern matching and used `wc -l` to count matching lines, applying the same pipe logic from this week's deeper notes (`command | grep pattern | wc -l`): each stage narrows down the previous stage's output until the final command gives a single, useful answer.

## What I practised overall

- Creating folders and files from scratch and confirming each step with `ls` or `cat`
- Running every command one at a time rather than pasting a whole block, to see what each one actually changed
- Copying vs. renaming vs. deleting, and understanding why each behaves differently
- Searching text content and counting results using the pipe pattern learned earlier this week

## Reflection

Running commands one at a time, instead of all at once, made it much easier to catch what each command actually did — especially the difference between `cp` (two files exist after) and `mv` (only one exists after), which looks similar on the surface but behaves very differently.

## Next step

Move to Lab 2: Permissions and users, applying the `chmod`/`chown` knowledge from this week's permissions notes hands-on.
