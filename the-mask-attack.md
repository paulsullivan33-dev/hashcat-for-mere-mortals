# The Mask Attack

A dictionary tries what someone might
have chosen. A mask attack tries
everything that *fits a shape* — four
digits, six lowercase letters and a
digit, a word's length with its pattern
of cases. It is the tool for chapter 2's
first honest job: the password you half
remember, whose shape you know exactly.

**Check you can run:** the command below
recovers the lab's PIN —
`fa8dbbcb682699544e4e8f2212115f73:4071` —
after testing all 10,000 four-digit
combinations (transcript t06).

## The command

```
$ ./hashcat.bin -m 0 -a 3 pin.md5 '?d?d?d?d'
```

Attack `3` is mask (older hashcat called
it brute-force; the mask is the modern
form). Each `?d` is one digit, 0–9. The
character sets:

| Code | Set | Size |
| --- | --- | --- |
| `?d` | digits `0-9` | 10 |
| `?l` | lowercase `a-z` | 26 |
| `?u` | uppercase `A-Z` | 26 |
| `?s` | symbols and space | 33 |
| `?a` | all of the above | 95 |
| `?b` | any byte | 256 |

Anything not written as a code is a
literal: the mask `pass?d?d` generates
`pass00` to `pass99` and nothing else.
Your own half-memory is usually a mask
with literals in it — a pet's name you
are sure of, plus the digits you are
not.

## Count before you run

The space is a product: four `?d` give
10 × 10 × 10 × 10 = 10,000 candidates.
Say the product out loud before every
mask run; it is the whole feasibility
question. The lab's run reported
`Progress.........: 10000/10000`, found
`4071`, and clocked about 38,000 H/s
even on this small CPU — a four-digit
PIN has no defence under a fast hash,
which is why real systems never store
PINs this way.

`--keyspace` prints a count without
running — with one quirk the lab hit
and kept: it counts the mask's outer
loop only. Hashcat splits a mask into a
base fed to the device and an amplifier
the device generates internally:

```
$ ./hashcat.bin -a 3 '?d?d?d?d' --keyspace
1000
```

1000 base × 10 amplifier = the 10,000
the status screen reports. Quote the
status screen's Progress for totals, and
do the product by hand for planning —
they must agree, and checking that they
do is free.

## How masks grow teeth

Each position multiplies. Adding one
`?a` multiplies the space by 95; moving
from `?d?d?d?d` (10⁴) to `?a?a?a?a?a?a`
(95⁶, about 735 billion) takes the same
run from a blink to hours on this CPU —
and to real time on a graphics card,
which is why "six characters, anything"
is not a password, it is a delay. The
hybrid attacks (`-a 6`, `-a 7`) soften
this: a wordlist word with a mask on one
end, for the extremely human pattern of
*a word I know plus digits I don't*.

Masks also accept custom charsets
(`-1`, `-2`, …) and incremental mode
(`--increment`, trying every length up
to the mask's). Both are refinements of
the same idea: shrink the space to the
shape of your actual knowledge, because
the shape is all a mask attack ever has.
