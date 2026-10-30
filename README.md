# Hashcat for Mere Mortals

A database leaks, and the headlines say
the passwords were "hashed", so they
were safe. A week later the same
passwords are circulating in plain text.
The machine that usually stands between
those two headlines is **hashcat**: a
program that recovers passwords from
their hashes by guessing — quickly,
methodically, and at a scale that turns
human password habits into arithmetic.

This guide explains how password hashes
work, why the weak ones fall and the
strong ones do not, and how to run
hashcat yourself: on your own machine,
against hashes you made or are
responsible for, for the legitimate
jobs — recovering a password you forgot,
auditing passwords on a system you run,
and learning from the inside why slow
salted hashes and long passphrases
matter. The ethics chapter is chapter 2,
before any procedure, and it is not
boilerplate: it is the licence the rest
of the guide operates under.

The lab that wrote this guide ran the
real thing: **hashcat 8.0.0**, the
official release binary, on a small
shared machine with **no graphics card**
— its two processor cores, through the
PoCL OpenCL runtime. Against hashes the
lab manufactured from invented passwords,
it ran a dictionary attack that
recovered two MD5 hashes, a mask attack
that enumerated all 10,000 four-digit
PINs, a rules attack that turned
`bluebird` into `Bluebird7`, and the
same dictionary attack against a salted
sha256crypt hash — which fell too, but
at 40 guesses a second instead of
hundreds of thousands. A benchmark pair
puts the honest number on the gap:
124 million MD5 guesses per second
versus 554 sha256crypt guesses per
second, on the same two cores. It also
kept its failures: hashcat with no
runtime visible at all, and a run
refused before it started because the
mode number was wrong.

What the lab could not do, it did not
fake. There was no graphics card, so GPU
speeds are the reader's to measure with
chapter 11's benchmark commands; the
ratios, not the rates, are the lesson.
Everything the guide claims as run is in
the transcripts; everything else is
labelled documented-only in
`criteria.md`.

## What this guide covers

- [**What Hashcat Is**](what-hashcat-is.md) —
  hashes as one-way fingerprints, why
  guessing is the only way back, and
  what the tool actually does
- [**The Rules: Your Hashes, Your Hardware**](rules-of-the-game.md) —
  the three tests every legitimate job
  passes, and the honest work this tool
  does
- [**The Moving Parts**](the-moving-parts.md) —
  hash files, mode numbers, attack modes,
  kernels and backends, salt, the potfile,
  sessions and the status screen
- [**Installing Hashcat**](installing-hashcat.md) —
  the release archive, the OpenCL runtime
  that is the real install, and the
  device check that proves it
- [**Your First Checks**](first-checks.md) —
  naming a hash type with `-m`, and
  reading a hash's shape with
  `--example-hashes`
- [**Making Practice Hashes**](making-practice-hashes.md) —
  manufacturing your own hashes and
  wordlist, and where real hashes sit in
  a shadow file
- [**The Dictionary Attack**](the-dictionary-attack.md) —
  the straight attack, two MD5 hashes
  recovered in one run, and why human
  habits are the real target
- [**Proving the Crack**](proving-the-crack.md) —
  three independent checks of a result,
  and how to read a status block like an
  auditor
- [**The Mask Attack**](the-mask-attack.md) —
  patterns and keyspaces: 10,000 PINs,
  counted before they are run
- [**Rules and Other Attacks**](rules-and-mutations.md) —
  mutations that model human decoration,
  plus combinator and hybrid attacks
- [**Slow Hashes Fight Back**](slow-hashes.md) —
  sha256crypt and the measured
  224,000-fold gap: the whole theory of
  password storage in two numbers
- [**Recipes**](recipes.md) — recovering
  your own forgotten password, auditing
  a system you run, and deciding whether
  a run is worth starting
- [**The Neighbours**](the-neighbours.md) —
  John the Ripper and its converters,
  hash identifiers, CyberChef, lookup
  websites, and the password managers
  that make this guide unnecessary
- [**Limits**](limits.md) — no reversing,
  no magic: shape, arithmetic, and the
  limits that are not technical
- [**Troubleshooting**](troubleshooting.md) —
  the missing runtime, the wrong mode,
  the misleading speed, and the potfile
  that remembers too well
- [**Glossary**](glossary.md) — the
  terms, collected in one place

## What it doesn't cover

Attacking anyone else's hashes, accounts
or systems — chapter 2 is the whole
position, and nothing here is written to
be repurposed. Capturing hashes in the
first place (network sniffing, Wi-Fi
handshakes, memory extraction) belongs
to other tools and other guides; this
one starts where a hash file already
legitimately exists. Distributed
cracking across many machines, and
hashcat's Brain and bridge-board
features, are named in passing at most.
Graphics-card tuning for maximum rate —
overclocking, driver archaeology — is a
sport of its own; chapter 11's benchmark
is enough to plan honest work.

## How to use this guide

Read chapters 1 to 3 before installing
anything: the model, the rules, and the
parts list turn every later error into a
named problem. Chapters 4 to 6 get you a
working tool and your own practice
material; chapter 7 makes the first
recovery in the same sitting. Chapter 8
is the one to read slowly — its three
checks are how you catch a wrong claim
before it becomes a forced password
reset. The lab's transcripts live in
`hashcat-lab/transcripts/`, and its
wordlists, rule file and practice hashes
live beside them in `hashcat-lab/`, so
every run in this guide can be repeated
exactly.

---

## Part of the series

This guide is one of the [For Mere Mortals](https://paulsullivan33-dev.github.io/mere-mortals/) guides: plain-language guides to the tools programmers and makers use.

Related guides:

- [Cybersecurity for Mere Mortals](https://paulsullivan33-dev.github.io/cybersecurity-for-mere-mortals/)
- [KeePassXC for Mere Mortals](https://paulsullivan33-dev.github.io/keepassxc-for-mere-mortals/)
- [pass for Mere Mortals](https://paulsullivan33-dev.github.io/pass-for-mere-mortals/)

## License

The writing in this guide is licensed under [CC BY 4.0](https://github.com/paulsullivan33-dev/hashcat-for-mere-mortals/blob/main/LICENSE). Code examples and lab scripts are licensed under the [MIT License](https://github.com/paulsullivan33-dev/hashcat-for-mere-mortals/blob/main/LICENSE-CODE).
