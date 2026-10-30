# What Hashcat Is

Someone steals a database of accounts, and
the news says the passwords were "hashed",
so everyone was safe. Then, a week later,
the same passwords are for sale in plain
text. What happened in between is, very
often, this program.

**Hashcat is a password recovery tool.**
You give it a **hash** — the scrambled
fingerprint a system stored instead of
your password — and it tries to find the
password that produced it. It does this
the only way anyone can: by guessing.
Guess a password, run it through the same
scrambling, and compare fingerprints.
Wrong guess, wrong fingerprint. Right
guess, and the fingerprints match exactly.

**Check you can run:** nothing to install
yet. Take the fingerprint
`8743b52063cd84097a65d1633f5c74f5`. By the
end of chapter 7 you will have recovered
the password behind it yourself, on this
machine, and in chapter 8 you will prove
the result three different ways.

## A hash is a one-way fingerprint

A **hash function** takes any input — a
letter, a password, a whole file — and
produces a fixed-size output called a
**hash** or **digest**. For MD5, the
output is always 32 hexadecimal
characters, whether the input was one
letter or a library.

Three properties matter for this guide:

- **One-way.** Given the hash, there is
  no formula that runs it backwards. The
  scrambling mixes the input so thoroughly
  that the only way back is to guess.
- **Deterministic.** The same input
  always gives the same hash. That is
  what makes guessing work — and what
  makes password storage possible at all.
- **Avalanche.** Change one letter of
  the input and the hash changes
  completely. There is no "close" — a
  guess is right or it is worthless.

Because of the first property, a hash is
not encryption. Encryption has a key and
can be undone by whoever holds it. A hash
cannot be undone by anyone. That is why
systems store hashes of passwords instead
of the passwords: to check your login,
the system hashes what you typed and
compares it with the fingerprint on file.
Your actual password never sits on disk.

## So why does guessing work?

Because people choose guessable
passwords. A hash function turns
`password123` into a fingerprint just as
faithfully as it turns a random
20-character string into one. The maths
does not know which input was weak. The
attacker's whole job is to order the
guesses well — try the passwords people
actually use, in the order people
actually use them — and to try them very,
very fast.

That is hashcat's contribution: speed
and organisation. It knows over 700
kinds of hash, it drives graphics cards
and processors at their full rate, and
it offers several ways of generating
guesses — from a list, by pattern, by
mutation — so a human's habits become a
search plan. On a fast graphics card it
can test billions of simple hashes per
second. Chapter 11 shows what happens
when the hash itself is built to be
slow: the same machine crawls, and that
crawl is the defence working as designed.

## What hashcat is not

- It does not "decrypt" anything. There
  is nothing to decrypt; there is only
  guessing that sometimes succeeds.
- It cannot recover a strong password.
  A long random password has more
  possible guesses than the universe has
  patience. Hashcat finds weak passwords
  because they are weak, not because the
  tool is magic.
- It does not break into accounts. It
  works offline, on hash files you
  already have. Where you got them, and
  whether they are yours to test, is the
  subject of chapter 2 — read it before
  you run anything.

The lab behind this guide ran hashcat on
a small shared machine with no graphics
card at all, using its processor alone,
against hashes the lab made itself from
invented passwords. Every recovery in
these chapters is real, and the
transcripts in `hashcat-lab/transcripts/`
are the evidence.
