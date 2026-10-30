# Proving the Crack

A password on hashcat's screen is a
claim. This chapter checks it three
independent ways, because the habit
matters more than any one result: in a
real audit, everything downstream —
forced resets, a report to management —
rests on these pairs being right.

**Check you can run:** `--show` prints
the recovered pairs, and hashing
`bluebird7` yourself with `md5sum`
reproduces
`1119a110df24a17a303d7fd89ee59c4c`
(transcript t04).

## First way: ask hashcat to repeat itself

```
$ ./hashcat.bin -m 0 md5.txt --show
8743b52063cd84097a65d1633f5c74f5:hashcat
1119a110df24a17a303d7fd89ee59c4c:bluebird7
```

`--show` does not crack anything. It
reads the potfile and prints the pairs
it holds for the hashes in your file.
It is the quick way to answer "what do
we already have?" at the start of any
session — and the reason a second
identical attack does no work:

```
$ ./hashcat.bin -m 0 -a 0 md5.txt words.txt
INFO: All hashes found as potfile and/or empty entries!
Use --show to display them.
```

The lookup happens before a single
kernel runs. If an audit's hash appears
here on day one, it fell in some earlier
session — yours, you hope.

## Second way: the potfile itself

```
$ cat hashcat.potfile
8743b52063cd84097a65d1633f5c74f5:hashcat
1119a110df24a17a303d7fd89ee59c4c:bluebird7
$5$labsalt42$JiFI...:trumpet99
fa8dbbcb682699544e4e8f2212115f73:4071
ccb113efbb2ac157a57b992ee08171c3:Bluebird7
```

(That is the lab's complete potfile at
the end of the guide, transcript t09 —
five lines, every one an invented
password.) The potfile is plain text:
hash, colon, plaintext. Nothing about
it is encrypted, because it is a working
notebook, not a vault. Two consequences:
back it up if a long audit matters, and
guard it like the passwords it contains
if the audit is real.

## Third way: check the maths yourself

The strongest proof uses no hashcat at
all. A hash is deterministic, so hash
the claimed answer with a tool you
already trust:

```
$ printf %s bluebird7 | md5sum
1119a110df24a17a303d7fd89ee59c4c  -
```

The fingerprint matches the one in the
hash file, character for character.
That is all a "crack" ever is: not the
hash reversed, but a candidate
demonstrated to reproduce it. For the
salted sha256crypt result the same check
is `openssl passwd -5 -salt labsalt42
trumpet99`, which rebuilds the identical
`$5$…` line from chapter 6.

## Reading a status block like an auditor

Chapter 7's closing block, line by line:

- **Status** — `Cracked` (all found),
  `Exhausted` (space searched, some or
  none found), or an interruption state.
- **Recovered … (new)** — the number
  that changed during *this* run. A run
  that recovers 5/100 with 0 new found
  nothing; it read the potfile.
- **Progress** — candidates tried out of
  candidates in the space. The honest
  denominator for every time estimate.
- **Speed** — measured on this run. On
  runs lasting seconds it measures
  startup, not cracking; benchmarks and
  long runs are the figures to quote.
- **Candidates.#01** — the first and
  last candidates of the current batch,
  a peek at where in the list or mask
  the search currently stands.
- **Restore.Point** — where a `--restore`
  would resume. Sessions checkpoint as
  they go; a power cut costs minutes,
  not the run.
