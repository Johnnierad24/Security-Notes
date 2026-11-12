# Linux Essentials: Key Directories

**Week:** 2
**Topic:** Linux fundamentals
**Date:** November 2026

## Why this matters

Linux runs most servers, security tools and SOC platforms. Knowing the file system layout means I can find configuration, logs and programs quickly, whether I'm hardening a system or investigating one.

## Key directories

| Path | What lives there |
|---|---|
| `/` | Root of the whole file system |
| `/home` | User folders (mine: `/home/kali`) |
| `/root` | Home folder of the administrator (root) |
| `/etc` | Configuration files |
| `/var/log` | Log files (vital for SOC work) |
| `/tmp` | Temporary files |
| `/bin`, `/usr/bin` | Programs and commands |
| `/dev`, `/proc` | Devices and running-process information |

## What I'll remember by analogy

Think of the file system like a building:
- `/` is the building itself.
- `/home` is everyone's private apartment.
- `/root` is the building manager's apartment (admin only).
- `/etc` is the building's rulebook and settings office.
- `/var/log` is the building's security camera footage and visitor logbook.
- `/tmp` is a shared table where people leave things temporarily, cleared out regularly.
- `/bin` and `/usr/bin` are the toolshed everyone can access.
- `/dev` and `/proc` are the building's utility meters and live activity boards.

## What I practised

- Navigated between these directories using `cd` and listed their contents with `ls -la`.
- Looked specifically inside `/etc` and `/var/log` to see real configuration and log files.

## Next step

Move on to core commands (`pwd`, `ls`, `cd`, `mkdir`, `cp`, `mv`, `rm`, `cat`, `grep`, `find`) and Labs 1 and 2 in the Master Guide.
