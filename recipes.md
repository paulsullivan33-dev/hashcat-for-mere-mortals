# Recipes

Four jobs this guide's chapters add up
to. All four assume chapter 2's tests
pass: the hashes are yours, the machine
is yours, the answers are yours to know.

## 1. Recover your own forgotten archive password

You remember it was a word, maybe
capitalised, possibly with a year on the
end — the classic half-memory.

1. Get the hash in hashcat's form. For
   many formats (zip, 7z, PDF, Office)
   a `*2john` helper script produces it;
   chapter 13 names the family.
2. Start with the dictionary you can
   build from your own habits: the words
   you actually reuse, written down.
3. Add rules before adding hours:
   `c`, `$7`-style appends, and the
   shipped `best64.rule` cover most
   human decoration.
4. Graduate to hybrid: your word list
   plus `-a 6` with a mask like `?d?d?d?d`
   for the forgotten digits.
5. Estimate first, always: `--keyspace`
   and the benchmark for that mode turn
   "let it run" into a number of hours
   you can choose on purpose.

## 2. Audit the passwords on a system you run

1. Export the hashes with the system
   owner's written permission — usually
   that is you — and keep the export and
   the results under the same care as
   the passwords themselves.
2. Begin with `--show`: anything already
   in your potfile from previous audits
   is a repeat offender.
3. Run a straight attack with a serious
   public wordlist, then a rules pass
   with `best64.rule`. On a healthy user
   base under a slow hash, expect a small
   minority to fall — those are the
   accounts to force-reset first.
4. Report rates, not plaintexts: how
   many fell, in which pass, and how
   fast. The report's job is to change
   policy (length, a blocklist, a better
   hash), not to shame a user by name.
5. Destroy or vault the recovered
   plaintexts when the resets are done.
   They are live credentials until then.

## 3. Test one stored hash before you trust the scheme

Migrating a login system? Hash one known
weak password and one known strong one
with the candidate scheme, then time a
dictionary run against each. You are
measuring chapter 11's factor on your
own terms: if the weak one falls before
your coffee cools, the scheme — not the
user — is the first thing to fix.

## 4. Decide whether a run is worth starting

The whole decision in three lines:

```
candidates = list length x rules (or the mask product)
seconds    = candidates / benchmark speed for the mode
```

If the answer is longer than the
password's likely lifetime of mattering,
change the *shape* of the attack — more
knowledge in the mask, a better-ordered
list — rather than the hardware. Every
chapter's failure in this guide was a
shape failure first and a speed problem
never.
