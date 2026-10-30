# The Dictionary Attack

The straight attack is the first tool
out of the box, and in real audits it
does most of the damage: hand hashcat a
list of likely passwords, and it tests
every one against every hash you gave
it.

**Check you can run:** with the lab's
files, the command below recovers both
MD5 hashes in one short run —
`8743b52063cd84097a65d1633f5c74f5:hashcat`
and
`1119a110df24a17a303d7fd89ee59c4c:bluebird7`
(transcript t03).

## The command

```
$ ./hashcat.bin -m 0 -a 0 md5.txt words.txt
```

Read it left to right: mode `0` (these
are MD5 hashes), attack `0` (straight —
the candidates come from a list, as
written), then the hash file, then the
wordlist. The lab's `md5.txt` holds two
hashes: the `--example-hashes` sample
from chapter 5, and the MD5 of
`bluebird7` made in chapter 6.

Hashcat parses the hashes, builds its
tables, compiles the MD5 kernel for the
device (once — it is cached afterwards),
checks its own work with a built-in
self-test, and runs. As each hash falls,
it prints the pair:

```
8743b52063cd84097a65d1633f5c74f5:hashcat
1119a110df24a17a303d7fd89ee59c4c:bluebird7
```

Then the closing status block:

```
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 0 (MD5)
Guess.Base.......: Feed (words.txt)
Speed.#01........:      536 H/s ...
Recovered........: 2/2 (100.00%) Digests (total), 2/2 (100.00%) Digests (new)
Progress.........: 48/48 (100.00%)
```

`Recovered 2/2` is the result; `Progress
48/48` says the whole list was needed as
the space; the run took seconds, nearly
all of it startup. Two lines of that
block deserve suspicion, and chapter 8
returns to them: the speed figure is
nearly meaningless on a 48-candidate
run, and "Cracked" describes these two
hashes, not the strength of anything
else.

## Why this works so embarrassingly well

Nothing in this run was clever. The
wordlist is 48 words, written in an
afternoon by imagining what people pick:
a colour and a bird with a digit, the
tool's own name. Real breaches fall the
same way, at bigger scale: the candidate
lists are longer and better ordered, but
the passwords were chosen by the same
species. A dictionary attack is not
really an attack on the hash function.
It is an attack on the password-choosing
habits of humans — and the hash function
merely keeps score, honestly and at
great speed.

Three practical consequences:

- **Order matters.** Hashcat works down
  the list; a well-ordered small list
  beats a huge shuffled one. Real lists
  are ordered by how often passwords
  actually occur.
- **Many hashes cost little more than
  one.** For unsalted fast hashes like
  MD5, each guess is checked against all
  the hashes at once. Ten thousand MD5
  hashes are barely harder than ten.
  Chapter 11 shows salt ending that
  bargain.
- **Exhausted is a result too.** If the
  list ends with nothing recovered, the
  status is `Exhausted`, not failure:
  you have proven those candidates are
  not the password, which is information
  an audit can use.

## A note on speed, before chapter 11

The `536 H/s` above is a measurement
artefact: the run spent its life
starting up and its 48 guesses finished
before the meter settled. The same
machine benchmarks MD5 at over
**100 million** guesses per second
(chapter 11, transcript t09). Judge a
setup by its benchmark and its big runs,
never by a run too small to measure —
a lesson the rules attack in chapter 10
demonstrates from the other side, when a
few hundred device-generated candidates
suddenly clock 373,700 H/s.
