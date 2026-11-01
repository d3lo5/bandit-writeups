
# Bandit Write-ups: Learning Linux From Absolute Zero

A public log of my progress on [OverTheWire Bandit](https://overthewire.org/wargames/bandit/),
a wargame that teaches Linux through the command line.

## Who I am and why this repo exists

I'm a self-taught beginner with **no prior experience in programming, Linux, IT or networking**.
I started Bandit on October 27, 2026, and on April 5, 2027 I start my cybersecurity
program at ISIL (Lima, Peru).

I'm learning as I go, so this repo includes my mistakes, not just the solutions.
Writing down why a command failed is how I check that I actually understand it.
I expect the mistakes to become fewer and more advanced over time. The
[mistake log](#mistake-log) below is how you can verify that.

I'm studying *The Linux Command Line* by William Shotts alongside the
levels. I'm currently on chapter 4.

## Rules I follow

- **No passwords or flags.** If you want to learn from Bandit, solve it yourself.
- **Honesty about help.** I use AI as a guide, not as an answer machine. I ask it to
  point me in a direction (for example, "which command compares file types?") rather than
  to give me the solution, because I'm not yet familiar with the commands.
  Each write-up states how much help I got.

## Progress

| Level | Key concept | Key command | Help received |
|-------|-------------|-------------|---------------|
| 4 → 5 | Filenames starting with `-` | `file ./*` | AI guidance on syntax |
| 5 → 6 | Searching by properties, hidden files | `find -size 1033c` | AI guidance on syntax |

---

## Level 4 → 5

**Goal:** find the only human-readable file in the `inhere` directory
(10 files: `-file00` to `-file09`).

**Solution:**
1. `ls -la` to list everything, including hidden files.
2. `file ./*` to check the type of every file. Only one reported `ASCII text`;
   the other nine were `data` (binary).
3. `cat ./-file07` to read it.

**Why it works:** `file` identifies what a file contains without opening it.
The `./` prefix tells the shell "this is a path in the current directory,"
so a leading `-` is not mistaken for an option.

**What went wrong:**

| What I typed | Why it failed |
|--------------|---------------|
| `file -file00` | The leading `-` is read as options (`-f`, `-i`, `-l`...), not as a filename |
| `cat /file00` | `/` is the filesystem root, not the current folder |

**Lesson:** if a filename starts with `-`, prefix it with `./`.

---

## Level 5 → 6

**Goal:** find a file under `inhere` that is human-readable, exactly 1033 bytes
and not executable. There are 20 directories with 6 files each.

**My first approach (slow, documented on purpose):**
I walked into each directory with `cd` and ran `file ./*` in every one.
I was reusing the level-4 method at the wrong depth. At the top level, `file`
only told me everything was a directory. Inside the folders, it listed file types
but couldn't tell me the size I needed, and I never saw the file because it
was hidden.

**What worked:**
1. `find -size 1033c` returned exactly one result: `./maybehere07/.file2`.
2. `ls -l .file2` confirmed 1033 bytes and permissions `-rw-r-----`
   (no `x`, so not executable).
3. `cat .file2` displayed the contents.

**The cleaner solution (learned afterwards):**
`find . -type f -size 1033c ! -executable`

**What went wrong:**

| What I typed | Result | Explanation |
|--------------|--------|-------------|
| `find -size 1033` | Nothing | Without a suffix, `-size` counts 512-byte blocks, not bytes |
| `find -size 1033M` | Nothing | `M` means megabytes |
| `find -size -1050M` | Everything | "Smaller than 1050 MB" matches every file here |
| `find -type 1033` | Error | `-type` takes a letter (`f`, `d`), not a number; `-size` is the test I needed |
| `file . /.file2` | Error | The space split it into two arguments, and `/.file2` looks in the root |
| `ls -size ...` | Error | `-size` is a `find` option, not an `ls` option |

I was also unsure whether I needed `file` or `cat` to read a file's contents.
They do different things: `file` tells you what a file is, `cat` prints what is inside it.
(Note: `cat .file2` and `cat ./.file2` both work.)

**Lessons:**
- In `find -size`, the suffix matters: `c` = bytes, `k` = KiB, `M` = MiB.
- Files starting with `.` are hidden; `ls` won't show them without `-a`.
- `/` (root), `./` (here) and a bare name are three different paths.
- `file` identifies, `cat` reads.
- The wildcard `*` does not match hidden files, so `file ./*` skipped `.file2` entirely.
---

## Mistake log

| Level | Mistake | Category |
|-------|---------|----------|
| 4→5 | `file -file00` | Leading dash read as option |
| 4→5 | `cat /file00` | Confusing `/` with `./` |
| 5→6 | `find -size 1033` | Wrong `find` size unit |
| 5→6 | `find -type 1033` | Wrong option value |
| 5→6 | `file . /.file2` | Path syntax (extra space, `/` vs `./`) |
| 5→6 | `ls -size ...` | Using one command's option in another |

*I'll keep adding rows. The goal is for this table to show fewer and more advanced errors over time.*

## Next

- Bandit to level 10, then 15.
- Shotts chapters 4 to 10, with a short summary of each in this repo.
- From level 7 onward, a "Help received" column will say "none" whenever it's true.
