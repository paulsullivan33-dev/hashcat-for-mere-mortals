# Your First Checks

Before cracking anything, learn the two
commands that answer the two questions
every later failure asks: *can hashcat
see hardware?* and *what kind of hash is
this?*

**Check you can run:** `./hashcat.bin -I`
shows your device, and
`./hashcat.bin -m 0 --example-hashes`
describes mode 0, MD5, with a sample hash
(transcript t02).

## Naming a hash type

Hashcat refuses to guess. Every run
names its hash type with `-m`, and the
types are numbered:

| Mode | Hash type | Where you meet it |
| --- | --- | --- |
| 0 | MD5 | old databases, checksums of passwords (never use for new storage) |
| 100 | SHA-1 | older systems, git object names |
| 1400 | SHA-256 | raw digests |
| 7400 | sha256crypt `$5$` | Linux `/etc/shadow` on many distributions |
| 1800 | sha512crypt `$6$` | Linux `/etc/shadow`, the common modern default |
| 3200 | bcrypt `$2*$` | web frameworks, OpenBSD |
| 22000 | WPA-PBKDF2-PMKID+EAPOL | captured Wi-Fi handshakes |

The number is a promise about the format
of every line in your hash file.
Chapter 15 shows what a broken promise
looks like: attacked as SHA-1, an MD5
hash fails before any guessing with
"Token length exception … 40 required
but only 32 entered" — the tool's own
hint that the mode is wrong.

## Reading a hash's shape

`--example-hashes` prints, for any mode,
a sample hash and the password that made
it:

```
$ ./hashcat.bin -m 0 --example-hashes
Hash mode #0
  Name................: MD5
  Example.Hash........: 8743b52063cd84097a65d1633f5c74f5
  Example.Pass........: hashcat

$ ./hashcat.bin -m 7400 --example-hashes
Hash mode #7400
  Name................: sha256crypt $5$, SHA256 (Unix)
  Example.Hash........: $5$7777657035274252$XftMj84MW...
  Example.Pass........: hashcat
```

Two habits follow from this. First,
before a real run, hold your hash next
to the example: same length? same
prefix? A sha256crypt hash starts `$5$`
and contains its salt in plain sight; an
MD5 hash is 32 hex characters and
nothing else. Second, the example pair
is a free smoke test — the rest of this
guide cracks that very MD5 example, so
its recovery in chapter 7 will tell you
your whole pipeline works.

## If you do not know the type

Sometimes a hash arrives with no label.
Shape narrows it a lot: length, alphabet
(hex only, or full base64-ish?), and
prefixes (`$5$`, `$6$`, `$2y$`, `$argon2`)
are fingerprints of the construction.
Tools named in chapter 13 (hashid and
friends) automate that guessing against
hundreds of formats. Treat their answer
as a shortlist, not a verdict: confirm
with `--example-hashes` before you run,
the way you would check a key against a
lock before forcing the door — except
here the check costs nothing and the
forcing is chapter 15's error message.
