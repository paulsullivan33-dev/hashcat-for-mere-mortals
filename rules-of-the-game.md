# The Rules: Your Hashes, Your Hardware

This chapter is the guide's licence to
exist, and it comes before any procedure
on purpose. Hashcat is dual-use: the same
command that lets you audit your own
password file lets a criminal work on a
stolen one. This guide teaches the first
job. Everything in it is written to be
useless for the second — small invented
fixtures, your own hardware, your own
hashes.

**Check you can run:** none. This is the
chapter you read, not the chapter you
type. If any later chapter's example
would not pass the three tests below,
that is a bug in the guide, not a
loophole for the reader.

## The three tests

**1. The hashes are yours.** They came
from a system you own, an account that is
yours, or a file you created for
practice. "I found a database online" is
not yours. "My employer gave me the
export and asked for an audit, in
writing" is yours, in your working hours,
for that purpose.

**2. The hardware is yours.** You run
hashcat on machines you own or are
explicitly allowed to load. A long
cracking run pins a processor or graphics
card at full power for hours; on someone
else's machine that is damage, not
research.

**3. The answer is yours to know.** You
are recovering a password you forgot, or
testing the strength of passwords you
are responsible for protecting. If
learning the plaintext would give you
access to something that is not yours —
someone else's account, someone else's
files — stop. Recovering it is the
break-in, whether or not you then "do
anything" with it.

Material that fails any test is not
auditing; in many places it is a crime,
and everywhere it is a violation of the
people the hashes describe. Hash files
from public breaches are other people's
credentials. This guide never uses them,
and neither should you.

## The honest work this tool does

- **You forgot a password** to your own
  encrypted archive, wallet, or document,
  and you remember roughly what it looked
  like. A mask attack (chapter 9) over
  your own memory of the pattern is the
  fastest legitimate recovery there is.
- **You run a system** and want to know
  whether your users' passwords would
  survive a stolen database. Cracking
  your own export, on your own hardware,
  and forcing a reset for everything that
  falls is a standard, respectable audit.
- **You are choosing defences.** Running
  the attacks in these chapters against
  hashes you made teaches, better than
  any slogan, why slow salted hashes and
  long passphrases matter. Chapters 11
  and 14 are built around that lesson.
- **You are learning** how password
  storage works, on practice material
  like this guide's lab.

## The lab's own practice

Every hash in this guide's lab was
manufactured in the lab, from a password
invented for the occasion, using commands
shown in chapter 6. The wordlist is
forty-odd invented entries written for
the lab. No real person's password, and
no hash of one, appears anywhere in this
repository — and the potfile in the
transcripts contains only the lab's own
recoveries.

One more rule, for later: when you audit
real systems, treat every recovered
plaintext as sensitive. It is a working
credential until it is reset. Store
results like passwords — because that is
what they are.
