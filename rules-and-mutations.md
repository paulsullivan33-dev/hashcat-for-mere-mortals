# Rules and Other Attacks

People rarely pick a bare dictionary
word. They pick a word and *do something
to it* — capitalise the first letter,
tack a digit on the end, swap an `a` for
a `4`. Rules teach the dictionary attack
those habits, so one list entry becomes
a family of candidates.

**Check you can run:** with the lab's
eight-line rule file, the run below
recovers `Bluebird7` from a wordlist
that contains only `bluebird`
(transcript t08).

## The command

```
$ ./hashcat.bin -m 0 -a 0 bluebird7-cap.md5 words-nodigit.txt -r lab.rule
ccb113efbb2ac157a57b992ee08171c3:Bluebird7
```

The target is the MD5 of `Bluebird7`.
The wordlist has `bluebird` — and,
deliberately, *not* `bluebird7`, which
the plain dictionary attack would have
found for free. Everything else is the
rule file's doing:

```
$ cat lab.rule
:
c
u
$1
$7
c$1
c$7
c$9
```

One rule per line, each a small program
read left to right:

- `:` — do nothing (the word as written)
- `c` — capitalise first letter, lower
  the rest
- `u` — uppercase everything
- `$1`, `$7` — append that character
- `c$7` — capitalise, *then* append 7:
  `bluebird` → `Bluebird` → `Bluebird7`

The status block shows the arithmetic:
47 words × 8 rules = 376 candidates,
`Progress 376/376`. It also shows the
speed lesson promised in chapter 7 —
because the rules run on the device as
an amplifier, the run finally had
enough parallel work to measure:
**373.7 kH/s**, against the 536 H/s the
tiny straight run reported. The machine
did not get faster. The work got shaped
so the meter — and the device — could
engage.

## The rule language in one paragraph

Rules can append and prepend (`$x`,
`^x`), delete and insert at a position,
replace characters (`sa4` turns every
`a` into `4` — the leetspeak swap),
reverse, duplicate, rotate, change case
per position, and reject candidates that
do not match a shape (not enough digits,
wrong length), so a mutated list is not
polluted with guesses the target format
forbids. Hashcat ships respected rule
files in its `rules/` folder —
`best64.rule` (64 high-yield mutations)
and `dive.rule` (tens of thousands) are
the famous ones. Bigger rule files are
bigger multipliers: wordlist length ×
rule count is always the space, so a
million-word list under `dive.rule` is
a run you plan, not a run you start on
a whim.

## The other two shapes of guessing

**Combinator (`-a 1`)** glues two lists:
every left word against every right
word. `blue` + `bird7` lives here, as do
the two-word passphrases people build
from a small personal vocabulary. The
space is the product of the two lengths
— two 10,000-word lists are 100 million
candidates before rules enter at all.

**Hybrid (`-a 6`, `-a 7`)** is a list
plus a mask — word then pattern, or
pattern then word. It is the combinator
idea with one side generated instead of
read, and it fits the commonest real
pattern of all: a remembered word and a
forgotten number.

All four attack shapes share one
principle, worth stating plainly because
chapter 14 leans on it: **they encode
knowledge about how the password was
chosen.** A run fails not when the
machine is slow, but when the truth is
outside the shape you described.
