# Bandit Write-ups: Learning Linux From Absolute Zero

A public log of my progress on [OverTheWire Bandit](https://overthewire.org/wargames/bandit/), a wargame that teaches Linux through the command line.

## Who I am and why this repo exists

I'm a self-taught beginner with **no prior experience in programming, Linux, IT or networking**. I started Bandit on October 27, 2026, and on April 5, 2027 I start my cybersecurity program at ISIL (Lima, Peru).

I'm learning as I go, so this repo includes my mistakes, not just the solutions. Writing down why a command failed is how I check that I actually understand it. I expect the mistakes to become fewer and more advanced over time. The [mistake log](#mistake-log) below is how you can verify that.

I'm studying *The Linux Command Line* by William Shotts alongside the levels. I'm currently on chapter 6.

## Rules I follow

- **No passwords, flags or solution walkthroughs.** OverTheWire's login banner asks players not to post passwords or spoilers, including write-ups of solutions. This repo documents my mistakes and the concepts I learned, not the answers.
- **Honesty about help.** Each level states exactly what help I got, including AI. From level 6 on I use a command reference guide, generated with AI and organized by level. It explains how commands work and does not give answers. On some levels I also asked AI a specific syntax question, and those are marked. These write-ups were drafted with AI assistance and edited by me.

## Update: November 15, 2026

I wanted to give an update on my current situation and explain why I haven't made any progress with my studies or the Bandit levels over the past week or so.

My regular working hours are normally from 8:00 AM to 6:30 PM. However, due to an increase in workload, since last week I've been working from 7:00 AM to 7:30 PM. By December, my schedule might extend even further, potentially from 7:00 AM to 9:00 PM.

As you can imagine, this has significantly affected my available time and energy. I haven't been able to dedicate the weekly hours I had planned for studying, practicing Linux, and progressing through OverTheWire Bandit.

However, I want to make one thing clear: **I'm not giving up, losing interest, or changing my commitment to this journey.** I understand that my workload is likely to increase over the coming months, but I'm determined to find a way to keep learning and making progress, even if I have to adjust my study schedule and work at a slower pace.

I see cybersecurity as a long-term career transition, and I'm committed to seeing it through. I might not be able to study as consistently as I'd like right now, but I'll keep showing up, learning, practicing, and moving forward.

Consistency over speed. That's the approach I'm taking.

## Progress

| Level  | Key concept                                      | Tools involved          | Help received |
| ------ | ------------------------------------------------ | ----------------------- | ------------- |
| 4 → 5  | Filenames starting with `-`                      | `file`, `cat`           | AI guidance on syntax |
| 5 → 6  | Searching by file properties, hidden files       | `find`                  | AI guidance on syntax |
| 6 → 7  | Searching the whole server, noisy errors         | `find`                  | AI guidance on one step (hiding error output); reference guide |
| 7 → 8  | Filtering lines instead of printing everything   | `grep`                  | AI explanation of `grep` syntax after a day of reading; reference guide |
| 8 → 9  | Duplicate detection only compares neighbor lines | `sort`, `uniq`, pipe    | Reference guide only (AI-generated); no hints on the solution |
| 9 → 10 | Extract first, filter second                     | `strings`, `grep`, pipe | Reference guide only (AI-generated); no hints on the solution |

---

## Level 4 → 5

**Goal:** find the only human-readable file in the `inhere` directory (10 files: `-file00` to `-file09`).

**What went wrong:**

I started by exploring the directory structure to identify the directory containing the files. Once inside, I listed its contents, including hidden files and additional file information.

I struggled a little with identifying and opening the correct file. I tried several approaches, but my command syntax was incorrect. After a few unsuccessful attempts, I asked Claude for some guidance, which helped me understand what I was doing wrong and how to approach the problem.

The key learning experience was understanding how to navigate directories, inspect their contents, identify files based on their actual content, and distinguish between filenames and file paths.

Eventually, I identified the file containing readable text and successfully retrieved the password for the next level.

What I learned:

Understanding directory navigation.

Listing files and inspecting directory contents.

Identifying file types and determining which files contain readable text.

Working with filenames that include special characters.

Understanding relative paths and how to reference files correctly.

Recognizing syntax errors and troubleshooting commands.

Takeaway: This level helped me become more comfortable navigating Linux directories and working with files. I also learned that understanding command syntax is just as important as knowing which command to use.

| What I typed   | Why it failed                                                               |
| -------------- | --------------------------------------------------------------------------- |
| `file -file00` | The leading `-` is read as options (`-f`, `-i`, `-l`...), not as a filename |
| `cat /file00`  | `/` is the filesystem root, not the current folder                          |

**Lesson:** if a filename starts with `-`, prefix it with `./`.

**Help received:** AI guidance on syntax.

---

## Level 5 → 6

**Goal:** find a file under `inhere` that is human-readable, exactly 1033 bytes and not executable. There are 20 directories with 6 files each.

**My first approach (slow, documented on purpose):** I walked into each directory with `cd` and ran `file ./*` in every one. I was reusing the level-4 method at the wrong depth. At the top level, `file` only told me everything was a directory. Inside the folders, it listed file types but couldn't tell me the size I needed, and I never saw the file because it was hidden.

**What went wrong:**

I started by exploring the directory structure to locate the directory containing the files. Once inside, I inspected its contents and began experimenting with different Linux commands to identify the file that met the challenge's requirements.

I used several approaches to investigate the files, their properties, and their characteristics. However, I struggled a little with the command syntax, particularly when trying to access and read files with unconventional filenames.

I tried several commands, but some of my attempts failed because I wasn't using the correct syntax. Eventually, I asked ChatGPT for guidance. It suggested an approach that was conceptually correct, although I initially implemented it with incorrect syntax.

After correcting the syntax, I successfully accessed the right file, retrieved the password, and completed Level 5.

What I learned

Navigating Linux directory structures.

Listing and inspecting directory contents.

Using search criteria to locate specific files.

Understanding file properties and identifying file types.

Working with relative paths and file references.

Understanding how Linux interprets filenames and command arguments.

Troubleshooting syntax errors and correcting command execution.

My current knowledge level and learning approach

I want to emphasize that I am a complete beginner in this field. I have no previous academic or professional background in programming, Linux, IT, computer systems, or networking.

Every new Bandit level introduces concepts, commands, and Linux functionality that I am still learning. Consequently, solving these challenges requires me to understand not only the objective but also the underlying commands and their syntax.

To build that foundation, I am complementing the practical exercises with The Linux Command Line by William Shotts. I am currently starting Chapter 4.

I understand that this is a gradual learning process. My objective isn't simply to obtain passwords and advance through the levels; I want to understand what I'm doing, why a particular command works, and how to apply that knowledge independently.

Final takeaway: Level 5 completed. Another step forward in developing my Linux fundamentals and troubleshooting skills.

| What I typed        | Result     | Explanation                                                                   |
| ------------------- | ---------- | ----------------------------------------------------------------------------- |
| `find -size 1033`   | Nothing    | Without a suffix, `-size` counts 512-byte blocks, not bytes                   |
| `find -size 1033M`  | Nothing    | `M` means megabytes                                                           |
| `find -size -1050M` | Everything | "Smaller than 1050 MB" matches every file here                                |
| `find -type 1033`   | Error      | `-type` takes a letter (`f`, `d`), not a number; `-size` is the test I needed |
| `file . /.file2`    | Error      | The space split it into two arguments, and `/.file2` looks in the root        |
| `ls -size ...`      | Error      | `-size` is a `find` option, not an `ls` option                                |

I was also unsure whether I needed `file` or `cat` to read a file's contents. They do different things: `file` tells you what a file is, `cat` prints what is inside it.

**Lessons:**

- In `find -size`, the suffix matters: `c` = bytes, `k` = KiB, `M` = MiB.
- Files starting with `.` are hidden; `ls` won't show them without `-a`.
- `/` (root), `./` (here) and a bare name are three different paths.
- `file` identifies, `cat` reads.
- The wildcard `*` does not match hidden files, so `file ./*` skipped hidden files entirely.

**Help received:** AI guidance on syntax.

---

## Level 6 → 7

**Goal:** find a file somewhere on the server with a given owner, group and size. ([Level page](https://overthewire.org/wargames/bandit/bandit7.html))

**What I tried and why it failed:**

| What I typed | Why it failed |
| --- | --- |
| `find -size 33c -group bandit5 -user bandit7` | Wrong group name (the level says `bandit6`), and `find` searches from the current directory by default, which was my empty home folder |
| `find ./size 33c` | Missing dash, and `./` made `find` treat `size` and `33c` as paths |
| `find ./-size 33c ./-group bandit5 ./-user bandit7` | I applied the `./` trick from 4→5 to **options**. `./` fixes filenames that start with `-`; it doesn't belong in front of options |
| `find ./*` | The glob expanded to nothing because my home folder is empty |
| `file ./-size 33c` | Wrong command: `file` identifies types, it doesn't search |
| `find -size 33 -user bandit7 -group bandit6` | Dropped the unit suffix. Without `c`, the number counts 512-byte blocks. Same mistake as 5→6 |
| Searching only `/bin` | Size alone matched 9 files there. One filter isn't enough, and I was searching in the wrong place |
| Searching from `/` | Hundreds of "Permission denied" lines buried the result |

**What happened, in my own words:**

I started by inspecting the directory contents and navigating through the Linux filesystem to understand where I needed to search.

The main challenge was locating a file based on specific ownership, group, and size requirements. I already had some familiarity with file-size search criteria, but this level introduced two additional search parameters related to file ownership and group membership.

I remembered these parameters from a Linux command reference document I had previously prepared with Claude while studying the commands required for the Bandit challenges.

I combined these criteria into a search operation, but encountered a large number of Permission denied messages. This initially confused me, and I struggled to identify the relevant result.

I eventually asked Claude for guidance. It suggested a way to redirect error messages so they wouldn't clutter the terminal output. After applying that approach, I was able to isolate the relevant file, inspect its contents, and successfully retrieve the password for the next level.


**Concepts I learned:**

Navigating the Linux filesystem, including the root and home directories.

Understanding file listings and file permissions.

Using multiple search criteria simultaneously.

Searching for files based on size, ownership, and group membership.

Understanding why permission errors occur during filesystem searches.

Learning how to redirect error messages and manage terminal output.

Using file inspection tools to retrieve information.

There is something important I realized while solving this level.

The file containing the password was actually among the results I had initially encountered, together with numerous other files that generated permission errors.

In hindsight, I could potentially have identified the correct file by patiently examining the results instead of immediately looking for external guidance.

I got a little frustrated by the amount of error messages and allowed that frustration to interrupt my troubleshooting process.

Next time, I want to be more patient, examine the output carefully, and exhaust my own possibilities before asking for help.

I also realized that having a command reference is extremely useful. I was able to remember and apply new search parameters because I had previously studied them in my personal Linux command notes.

This level taught me more than just another Linux command. It reinforced the importance of understanding file ownership, group permissions, filesystem navigation, and command-line troubleshooting.

I also learned that encountering errors doesn't necessarily mean I'm doing something wrong. Sometimes, it's simply a matter of understanding what the system is telling me and knowing how to interpret the output.


**Help received:** I got stuck on the error flood and asked AI how to hide it. That was the one step I asked for. I also used the reference guide. The real mistakes came earlier: the wrong group name and the wrong search location.

---

## Level 7 → 8

**Goal:** find a specific word in a large text file (4,184,396 bytes, about 4 MB). ([Level page](https://overthewire.org/wargames/bandit/bandit8.html))

**What I tried and why it failed:**

| What I typed | Why it failed |
| --- | --- |
| `cat data.txt` | Printed the whole 4 MB file. Right tool for reading a file, wrong tool for finding one line in it |
| `cat grep millionth data.txt` | `cat` treats every argument as a filename. It printed the file, then complained that `grep` and `millionth` don't exist |

**What happened, in my own words:**

Something different about this level was how I approached it.

Before attempting the challenge, I dedicated some time during the day to studying two Linux utilities: sort and grep.

I wanted to understand their purpose, functionality, and practical applications before using them in Bandit.

Although I understood the general concepts, I still struggled to construct the commands with the correct syntax and apply them to the challenge.

Eventually, I asked Claude a more specific question in the conversation I've been using to study Linux commands. I focused particularly on understanding how grep works and how to apply it to the Level 7 → Level 8 challenge.

That explanation helped me connect what I had been reading with the practical exercise.


**Concepts I learned:**

Inspecting files containing large amounts of text.

Understanding why manually reading large files is inefficient.

Learning the purpose and functionality of grep.

Exploring the sort utility and its applications.

Searching for specific information within text files.

Understanding command syntax and how to apply it to practical problems.

Connecting theoretical knowledge with hands-on Linux exercises.

I'm starting to understand that completing Bandit isn't simply about finding passwords or advancing through levels.

Each challenge introduces new Linux concepts, utilities, and ways of interacting with the operating system.

Taking time to study these concepts before attempting the challenges is helping me build a stronger foundation.

I still need external guidance when I struggle with syntax or command construction, but I'm gradually becoming more familiar with the tools and understanding how they work.

This level reinforced the importance of learning the tools before trying to use them.

I am beginning to recognize commands, understand their purpose, and identify situations where they might be useful. However, translating that knowledge into correctly structured commands remains a learning curve.

My objective is to become increasingly independent, relying less on external guidance as my Linux knowledge improves.


**Help received:** after a day of reading about `grep` and `sort`, I asked AI a specific question about how to build the `grep` command. I didn't ask for the answer, but the syntax explanation was the help. I also used the reference guide.

---

## Level 8 → 9

**Goal:** find the only line in a file that occurs exactly once. ([Level page](https://overthewire.org/wargames/bandit/bandit9.html))

**What I tried and why it was wrong:**

| What I typed | Why it was wrong |
| --- | --- |
| `sort -n data.txt` | `-n` is a numeric sort and my data wasn't numbers. It worked only by luck: lines that tie numerically are then compared as whole lines, so identical lines still ended up adjacent. I re-ran without it and it was never needed |

This level introduced two Linux commands that were completely new to me: sort and uniq.

I went back to my Bandit command reference guide to understand their purpose, functionality, and available options.

I understood that I needed to organize and examine the information in the provided file, but I wasn't sure which options or variations of the commands I should use.

I initially experimented with different options for sort, including numerical sorting. However, after examining the output, I realized that I was making the process unnecessarily complicated. The basic functionality of the command was sufficient for this challenge.

The next step was understanding how to identify information that appeared only once. This led me to explore the uniq command, particularly its ability to display lines that are not repeated.

After studying some examples and experimenting with the syntax, I managed to combine both commands into a single operation using a Linux pipe.

Eventually, I obtained the correct result and successfully retrieved the password for the next level.

**Concepts I learned:**

Understanding the purpose and functionality of sort.

Exploring different sorting options and variations.

Understanding how uniq identifies repeated and non-repeated lines.

Learning how to identify unique information within a text file.

Combining multiple commands into a single operation.

Understanding the basic functionality of Linux pipes (|).

Recognizing when additional command options are unnecessary.

My biggest challenge: Understanding Linux pipes

One concept I still don't fully understand is when and how to use the pipe operator (|).

I understand that it allows me to connect commands, passing the output of one command as input to another. However, I'm still learning when this approach is necessary, how to determine which commands can be combined, and how to structure the syntax correctly.

This is something I want to explore further because I can already see how useful it is when working with large amounts of information.

This level was particularly interesting because it introduced me to a different way of thinking about command-line operations.

Instead of executing commands independently, I started learning how to combine Linux utilities to accomplish a task more efficiently.

I also realized that using more options doesn't necessarily mean getting better results. Sometimes, understanding the basic functionality of a command is all that's required.

I'm still learning the fundamentals, and concepts such as pipes and command combinations will require more practice.

**Help received:** reference guide only (AI-generated). No hints on the solution.

---

## Level 9 → 10

**Goal:** find a human-readable string in a mostly binary file. ([Level page](https://overthewire.org/wargames/bandit/bandit10.html))

**What I tried and why it failed:**

| What I typed | Why it failed |
| --- | --- |
| `grep = data.txt \| strings -excecutable` | I invented an option from an English word and misspelled it. `strings` printed its valid options and I could have read them |
| `grep = data.txt` | `grep` refused to print lines from a binary file: "binary file matches" |
| `strings -n = blob.bin` | `-n` takes a number (minimum string length), not a pattern. And `blob.bin` doesn't exist; my own `ls` showed only `data.txt` |

This level introduced me to working with binary files and extracting human-readable information from them.

I started by inspecting the available files and experimenting with commands I had already encountered, particularly grep and strings.

My initial approach was to use grep to search directly through the file, assuming I could identify the relevant information without any additional processing. However, this didn't work as expected because the file was recognized as binary rather than a regular text file.

I then explored the strings command, which extracts readable character sequences from binary files. I understood that this was probably the right direction, but I struggled with some of its options and initially used incorrect syntax.

After a few unsuccessful attempts, I reconsidered my approach and realized I could combine strings with grep using a Linux pipe.

That was the breakthrough. I managed to extract the readable information, filter the results, identify the relevant entry, and successfully retrieve the password.

**Concepts I learned:**

Understanding the difference between text files and binary files.

Learning the purpose and functionality of strings.

Understanding why grep behaves differently when searching binary files.

Extracting human-readable information from binary data.

Using grep to filter information based on a specific pattern.

Combining commands using Linux pipes (|).

Understanding how the output of one command can become the input of another.

Recognizing the importance of command syntax and options.


My biggest takeaway: Understanding how commands work together

This level helped me develop a better understanding of Linux pipes.

In previous challenges, I started experimenting with combining commands, but I wasn't entirely comfortable with the concept.

Now I'm beginning to understand that different commands can perform different stages of the same operation.

For example, one command can extract information, while another filters that information to identify exactly what I'm looking for.

I also learned that strings is particularly useful when dealing with binary files because it allows me to extract readable character sequences that would otherwise be difficult to inspect.

I'm starting to recognize some standard command combinations, although I still need to learn which commands can be connected through pipes and how to structure those combinations correctly.


This level reinforced the importance of experimenting, understanding error messages, and adapting my approach when something doesn't work.

I initially tried to solve the challenge using a single command, but eventually realized that combining two utilities was a much more effective approach.

I'm gradually moving beyond simply learning individual Linux commands and starting to understand how they interact with one another.


**Help received:** reference guide only (AI-generated). No hints on the solution.

---

## Mistake log

| Level | Mistake                                  | Category                                              |
| ----- | ---------------------------------------- | ----------------------------------------------------- |
| 4→5   | `file -file00`                           | Leading dash read as option                           |
| 4→5   | `cat /file00`                            | Confusing `/` with `./`                               |
| 5→6   | `find -size 1033`                        | Wrong `find` size unit                                |
| 5→6   | `find -type 1033`                        | Wrong option value                                    |
| 5→6   | `file . /.file2`                         | Path syntax (extra space, `/` vs `./`)                |
| 5→6   | `ls -size ...`                           | Using one command's option in another                 |
| 6→7   | `find -size 33c -group bandit5 ...`      | Wrong group name; wrong search directory              |
| 6→7   | `find ./-size 33c ./-group ...`          | `./` applied to options (misapplied lesson from 4→5)  |
| 6→7   | `file ./-size 33c`                       | Wrong command for the task                            |
| 6→7   | `find -size 33 -user bandit7 ...`        | Dropped the unit suffix (repeat of 5→6)               |
| 7→8   | `cat grep millionth data.txt`            | Passed a command as an argument to `cat`              |
| 8→9   | `sort -n data.txt`                       | Guessed option, not needed (worked by luck)           |
| 9→10  | `strings -excecutable`                   | Invented, misspelled option                           |
| 9→10  | `grep = data.txt`                        | Searching a binary file with `grep` directly          |
| 9→10  | `strings -n = blob.bin`                  | Pattern in a numeric option; filename not checked     |

*I'll keep adding rows. The goal is for this table to show fewer and more advanced errors over time.*

## Next

- Bandit to level 15.
- Shotts chapters 6 to 10, with a short summary of each in this repo.
- The "Help received" column states exactly what help I got on each level, including AI.
