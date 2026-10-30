# Making Practice Hashes

Every hash in this guide was made by the
lab, from a password invented for the
occasion, with the commands in this
chapter. Making your own is the only way
to practise that passes chapter 2's
tests — and it takes less than a minute.

**Check you can run:** hash a word with
`md5sum`, and get the same 32 hex
characters the chapter shows
(transcript t02).

## Three small factories

**MD5** comes from any system:

```
$ printf %s bluebird7 | md5sum
1119a110df24a17a303d7fd89ee59c4c  -
```

`printf %s` prints the word with no
trailing newline — a newline is a
character too, and hashing `bluebird7\n`
gives a different fingerprint. The `-`
is md5sum's way of saying "the input was
standard input". Keep the hex, drop
the rest.

**sha256crypt**, the salted slow kind,
comes from `openssl`:

```
$ openssl passwd -5 -salt labsalt42 trumpet99
$5$labsalt42$JiFI1KKMJivdPzkRywVZ8TW8abH6zCIp2yp.QjcuPT9
```

Read the output as its parts: `$5$` says
sha256crypt, `labsalt42` is the salt
(yours will be random if you omit
`-salt`, which is what real systems do —
the lab fixed its salt so this guide's
transcripts are repeatable), and the
long tail is the digest. The whole line,
salt and all, is the hash you hand to
hashcat: the salt is not a secret, and
the attacker needs it to compute guesses
at all.

**A PIN**, for chapter 9, is just a
password that happens to be digits:

```
$ printf %s 4071 | md5sum
fa8dbbcb682699544e4e8f2212115f73  -
```

## Where real hashes sit

On a Linux machine, password hashes live
in `/etc/shadow`, one line per user,
fields separated by colons:

```
practice:$5$labsalt42$JiFI...:19800:0:99999:7:::
```

Field 2 is the hash — `cut -d: -f2`
takes exactly it (transcript t10). The
file is readable only by root, which is
the operating system enforcing chapter
2's first test for you. Web databases
store hashes in a password column with
the same `$5$…`, `$2y$…` shapes, which is
why `--example-hashes` remains the field
guide even off Unix.

## The lab's wordlist

Attacks need candidates as well as
hashes, so the lab wrote one:
`hashcat-lab/words.txt`, 48 invented
entries — colours, animals, objects, a
few with digits glued on, in the shape
of the weak passwords real people pick.
It contains, unlabelled, the three
passwords this guide recovers by
dictionary (`hashcat`, `bluebird7`,
`trumpet99`). Real audits use lists with
millions of entries, assembled from
public password research; the mechanics
are identical, only the waiting differs.

Never build practice material the other
way round — by downloading a breach
dump "just to look". The hashes in it
are real people's credentials, however
old, and chapter 2's tests do not have a
curiosity exception.
