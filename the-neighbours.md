# The Neighbours

Hashcat is not the only tool at this
table, and for some hashes it is not
even the first one to reach for. These
neighbours are characterised from their
documentation; the lab did not run them.

## John the Ripper — the older brother

John predates hashcat and remains its
peer. Where hashcat is built around
device speed and numbered modes, John is
built around formats and a famous set of
incremental modes that generate
candidates from letter statistics. Its
practical gift to hashcat users is the
`*2john` family of small converters —
`zip2john`, `pdf2john`, `office2john`,
`sshkey2john` and dozens more — which
extract a hashcat-ready hash line from
an encrypted file. A common workflow is
bilingual: extract with John's helpers,
crack with whichever engine suits the
hardware. John's own cracking is
excellent, especially on CPU-only
machines and odd formats; many auditors
keep both and choose per job.

## hashid and Name-That-Hash — the identifiers

Given an unlabeled hash, these tools
match its shape against hundreds of
known formats and return a shortlist —
hashid prints the hashcat mode numbers
directly. They answer chapter 5's
question mechanically. Their limit is
chapter 5's warning: shapes collide
(several constructions share a length
and an alphabet), so confirm the winner
against `--example-hashes` before
running anything long.

## CyberChef — the workbench next door

GCHQ's browser workbench (there is a
guide to it in this series) is where a
single mystery string gets decoded,
re-encoded and identified interactively.
It computes hashes too, one at a time —
the opposite end of the scale from
hashcat's billions, and the right tool
when the question is "what *is* this?"
rather than "what made this?".

## The websites that offer to crack it for you

Search engines will offer online
"MD5 decrypters" and hash lookup
services. Understand what they are: a
potfile with a web page — a database of
hash-and-plaintext pairs someone else
already recovered. Looking up a hash
there means *telling a stranger the
hash*, and for anything but the most
public example values — chapter 1's
`8743b520…` is in every one of them —
that is handing out a credential
fingerprint. For real hashes, the answer
is this guide's rule: your hashes, your
hardware, no uploads.

## Password managers — the preventive neighbours

KeePassXC and pass (both in this series)
belong in this chapter too, because
their job is to make this entire guide
unnecessary for the passwords you
personally use: long, random, unique
per site, stored encrypted under one
strong passphrase. Hashcat's honest
finding, after all the benchmarks, is
that a generated 16-character password
is outside every attack shape in this
book. The best use of a cracking tool,
most days, is proving that your own
house is the one it cannot enter.
