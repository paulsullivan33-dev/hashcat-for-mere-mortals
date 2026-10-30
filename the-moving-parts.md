# The Moving Parts

Hashcat looks like one command, but it
helps to know which part does what,
because every failure later in this guide
belongs to exactly one of them.

**Check you can run:** none yet — this
is the map. By chapter 5 you will have
touched every part named here.

## The hash file

One hash per line, in a plain text file.
That is the whole format for most modes.
Salted and structured hashes carry their
extra pieces inside the line itself, in a
notation the mode defines — a Unix
sha256crypt hash, for instance, begins
`$5$`, then the salt, then the digest,
all in one string. Hashcat never asks
"which user is this?": a bare hash file
has no names in it, and chapter 6 shows
how to build one from a shadow-style
file when you need to.

## The mode number: `-m`

Hashcat knows over 700 hash types, and
it will not guess which one you mean
(except in narrow, flagged cases). You
name the type with `-m` and a number:
`0` is MD5, `100` is SHA-1, `7400` is
sha256crypt, `3200` is bcrypt. The number
is a promise — "every line in this file
is this kind of hash" — and a wrong
promise is the most common beginner
failure in this guide (chapter 15 shows
one, verbatim). `--example-hashes` prints
a sample of every mode, which is how you
check a hash's shape against the mode
before you spend a second cracking.

## The candidates, and the attack mode: `-a`

The guesses have to come from somewhere.
The **attack mode** says where:

- `-a 0`, **straight**: read candidates
  from a wordlist, one per line.
- `-a 1`, **combinator**: glue two
  wordlists together, left word + right
  word.
- `-a 3`, **mask**: generate candidates
  from a pattern, such as `?d?d?d?d` —
  four digits, every combination.
- `-a 6` and `-a 7`, **hybrid**: a
  wordlist word with a mask attached at
  the end or the start.
- **Rules** are a modifier, not a mode:
  in a straight attack, `-r` applies a
  file of small mutations — capitalise,
  append a digit, swap letters for
  lookalikes — to every wordlist word.

Choosing among these is the real skill
of the tool. The engine is fast, but no
engine outruns a search space that was
never going to contain the password.

## The engine room: backends and kernels

For each mode, hashcat carries a small
program — a **kernel** — that computes
that hash as fast as the hardware
allows. Kernels run on a **backend**:
OpenCL, CUDA, HIP or Metal. A **device**
is one piece of hardware behind a
backend: a graphics card, or — as in
this guide's lab — a processor, through
a CPU OpenCL runtime. `hashcat -I` lists
the devices it can see; if it sees none,
nothing else in the tool matters, which
is why chapter 4 spends as long on the
runtime as on hashcat itself.

Kernels are compiled the first time a
mode meets a device, and the compiled
form is cached on disk. First runs are
therefore slower to start than second
runs — a delay the transcripts in this
guide's lab show honestly.

## Salt: the second ingredient

Many hashes are computed over the
password **plus a salt** — a random value
stored beside the hash. The salt means
two users with the same password get
different hashes, and it means an
attacker cannot test one guess against a
million hashes at once: each salt needs
its own run of guesses. Salt is why
"Hashes: 1 unique salts" in the status
output is a number worth reading.
Chapter 11's slow mode is salted as well
as slow, and you can feel both effects.

## The potfile: the memory

Every password hashcat recovers is
appended to a **potfile**. The release
archive keeps it inside the hashcat
folder itself, as `hashcat.potfile`;
packaged installs keep it under
`~/.local/share/hashcat/`. Either way,
you can point elsewhere with
`--potfile-path`. The
potfile is why a second run over the same
hash finishes instantly: the answer is
looked up, not recomputed. `--show`
reads the potfile and prints the pairs it
holds for your hash file. It is also, be
clear, a file full of working passwords:
chapter 2's handling rule applies to it.

## Sessions and the status screen

A long run is a **session**: hashcat
saves progress as it goes, and `--restore`
resumes after an interruption. While a
run proceeds, pressing `s` prints a
status block — speed in hashes per
second, progress through the candidate
space, how many digests are recovered,
and an estimate of the time left. The
estimate is arithmetic, not prophecy:
progress divided by space, at the speed
measured so far. Chapter 8 reads one of
these blocks line by line.
