# Linux Core Commands, Pipes/Redirection, and Permissions

**Week:** 2
**Topic:** Linux fundamentals
**Date:** November 2026

## Why this matters

Almost every security tool, log, and SOC workflow runs through a Linux terminal. Today's topics are the three skills that make the terminal actually usable: combining commands, moving data between them, and controlling who can touch what.

## Core commands and flag-combining

A dash followed by several letters is shorthand for several single-letter flags typed as one block.

```
ls -la   =   ls -l -a
```

`-l` means long format (show details), `-a` means show hidden files too. Flags can be written separately or combined — both work the same.

## Pipes: sending one command's output into another

A **pipe** (`|`) sends the output of one command straight into the next, like connecting pipes on a factory line. Each command is a worker station; whatever one produces lands on the belt and goes straight to the next worker.

**How to read any pipe, step by step:**
1. Find each `|` and mentally split the line into separate commands.
2. Walk through **left to right**: picture the first command's output as a list of text lines, then feed that list into the next command, and so on.
3. The **last** command in the chain tells you the final shape of the answer (a count, a sorted list, a filtered list).

### Worked example: counting failed log lines

```bash
cat /var/log/syslog | grep -i fail | wc -l
```

| Step | Command | What it does |
|---|---|---|
| 1 | `cat /var/log/syslog` | Prints the entire log file |
| 2 | `grep -i fail` | Keeps only lines containing "fail" (`-i` ignores case) |
| 3 | `wc -l` | Counts how many lines survived (`wc` = word count, `-l` = count lines) |

**Answer:** how many lines in that log mention "fail".

## Redirection: sending output to a file instead of a command

Redirection looks similar to a pipe but does something different — instead of feeding another **command**, it feeds a **file**.

| Symbol | Meaning |
|---|---|
| `>` | Send output to a file, **overwriting** it |
| `>>` | Send output to a file, **appending** to the end |

```bash
ls -la > files.txt      # overwrites files.txt with the listing
ls -la >> files.txt     # adds the listing to the end of files.txt
```

**Key distinction I learned today:** `ls -la > files.txt` has no `|` at all, so it is **not** a pipe — it's redirection. A pipe connects two commands; redirection connects a command to a file.

## Why `sort` has to run before `uniq`

```bash
sort names.txt | uniq -c | sort -nr
```

| Step | Command | What it does |
|---|---|---|
| 1 | `sort names.txt` | Prints the lines alphabetically sorted |
| 2 | `uniq -c` | Collapses **consecutive** duplicate lines into one, counting occurrences |
| 3 | `sort -nr` | Sorts those counts numerically (`-n`) and reverses the order (`-r`), biggest first |

**The key insight:** `uniq` can only spot duplicates that sit **next to each other** in the list. If the file isn't sorted first, identical names scattered throughout the file would never be recognized as duplicates. Sorting first guarantees every copy of the same name ends up adjacent.

**Answer:** which names appear most often, most-common first.

## Permissions: reading the 10-character string

Running `ls -l` shows something like `-rwxr-xr--`. Picture this as **10 boxes**:

- **Box 1** = file type (`-` = regular file, `d` = directory, `l` = symbolic link)
- **Boxes 2-4** = **owner**
- **Boxes 5-7** = **group**
- **Boxes 8-10** = **others**

This order is fixed — always owner, then group, then others — the same way a phone number's area code always sits in the same position regardless of what the digits are.

Inside each group of three, position always means the same thing:

| Position | Letter | Meaning |
|---|---|---|
| 1st | `r` | read |
| 2nd | `w` | write |
| 3rd | `x` | execute |

A `-` in any position means that permission is off.

**Example:** `-rwxr-xr--` → owner `rwx` (full control), group `r-x` (read + run, no edit), others `r--` (read only).

## Converting letters to numbers

Each letter has a fixed point value:

| Letter | Value |
|---|---|
| `r` | 4 |
| `w` | 2 |
| `x` | 1 |

Because 4, 2, and 1 are powers of two, every possible combination adds up to a **unique** total — so one number always converts back to exactly one set of letters.

**Converting `-rwxr-xr--`:**
- Owner: `rwx` = 4+2+1 = **7**
- Group: `r-x` = 4+0+1 = **5**
- Others: `r--` = 4+0+0 = **4**

So this file is **754**.

| Number | Letters | Use |
|---|---|---|
| 755 | owner `rwx`, group `r-x`, others `r-x` | Scripts/programs — everyone can run it, only owner edits it |
| 644 | owner `rw-`, group `r--`, others `r--` | Normal files — nobody needs to execute a document |
| 600 | owner `rw-`, group `---`, others `---` | Private files — only the owner can even look |

## `chmod` and `chown`

`chmod` = **ch**ange **mod**e (permissions). Two ways to use it:

- **Numeric:** `chmod 600 secret.txt` — states the entire final permission set
- **Symbolic:** `chmod +x script.sh` — states only the **change** to make, leaving the rest untouched

Symbolic mode uses "who" letters: `u` = owner, `g` = group, `o` = others, `a` = all (default). Combined with `+` (add), `-` (remove), `=` (set exactly):

```bash
chmod o+x script.sh       # add execute, others only
chmod g+w file            # add write, group only
chmod go-rwx secret.txt   # remove everything from group and others
```

`chown` is a different tool entirely — `chmod` answers "what can be done?", `chown` answers "**who owns it**?" It needs `sudo` because reassigning ownership is a sensitive admin action.

```bash
sudo chown kali:kali file   # kali becomes owner and group
```

## What I practised

- Ran `ls -la` and manually converted several files' permission strings to numbers and back
- Built and tested pipe chains one command at a time, checking the output at each stage
- Tested the difference between `>` and `>>` on a practice file
- Used `chmod` in both numeric and symbolic form, and `chown` with `sudo`

## Next step

Continue with Labs 1 and 2 hands-on in Kali if not already finished, then move toward core networking commands and Wireshark (Lab 3).
