# Slow Hashes Fight Back

Everything so far fell in seconds,
because MD5 is built for speed — it was
designed to checksum data quickly, and
password storage borrowed it. This
chapter runs the identical dictionary
attack against a hash designed to be
slow, and measures the difference on the
same machine. The difference is the
entire modern theory of password
storage, in two numbers.

**Check you can run:** the benchmark
commands at the end print your machine's
own pair of numbers (transcript t09).

## The same attack, a different world

```
$ ./hashcat.bin -m 7400 -a 0 trumpet99.sha256crypt words.txt
$5$labsalt42$JiFI1KKMJivdPzkRywVZ8TW8abH6zCIp2yp.QjcuPT9:trumpet99
```

It worked — `trumpet99` was in the list
— but read the status block against
chapter 7's:

```
Hash.Mode........: 7400 (sha256crypt $5$, SHA256 (Unix))
Speed.#01........:       40 H/s ...
Restore.Point....: 0/48 (0.00%), Iterations:5000
```

Two defences are visible. **Iterations:
5000** — sha256crypt hashes its own
output five thousand times, so every
single guess costs 5000 times the work.
And the salt (`labsalt42`, carried in
the hash) means the guesses were
computed for this one hash alone; a
second user's hash, even of the same
password, would need the whole run
again. The 48-word list still finished,
because 48 is nothing. That is the
point: slow hashes do not make weak
passwords safe, they make *searching*
expensive — every extra candidate a
real audit needs (millions, billions)
now has a price tag.

## The benchmark pair

Hashcat's built-in benchmark, `-b`,
feeds each mode a full workload with
its optimised kernel:

```
$ ./hashcat.bin -b -m 0
Speed.#01........:   124.2 MH/s
$ ./hashcat.bin -b -m 7400
Speed.#01........:      554 H/s
```

On the same two processor cores:
**124 million MD5 guesses per second,
554 sha256crypt guesses per second** —
a factor of about 224,000. (The lab ran
the pair twice on the shared machine
and got 113.7 MH/s and 499 H/s the
first time; speeds are ranges, and the
factor barely moved.) A billion
candidates is nine seconds of MD5. It
is three weeks of sha256crypt. Same
attacker, same hardware, same list —
the only difference is what the defender
chose to store.

bcrypt (mode 3200) pushes further: its
cost is set by an exponent, so a
defender can raise it as hardware
improves, and its design is deliberately
awkward for graphics cards. Argon2 adds
memory hunger — each guess needs a
large block of RAM, and a graphics card
with thousands of cores cannot give
every core a large block. The direction
of fifty years of password storage is
all in this chapter's two numbers:
make the honest check cheap enough
(554 per second is plenty for logging
in) and the dishonest billion checks
ruinous.

## What this means for your passwords

- A password stored as MD5 is protected
  by exactly one thing: its own
  unguessability. Nothing else stands
  between it and 124 million guesses a
  second on a borrowed laptop CPU — a
  real graphics card goes far faster.
- A password stored as sha256crypt,
  bcrypt or Argon2 can survive being
  *ordinary* — but not being *known*.
  `trumpet99` fell instantly: it was in
  the list. Slowness buys time against
  search, not immunity against the
  password being a common choice.
- Length beats cleverness at both
  speeds. Every extra character
  multiplies the mask space; no rule
  file multiplies as reliably as one
  more character of passphrase.
