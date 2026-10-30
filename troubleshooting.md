# Troubleshooting

The failures this guide's lab actually
met, plus the ones every new user meets,
in roughly the order you will meet them.

## "No OpenCL, HIP or CUDA compatible platform found"

Hashcat sees no hardware at all
(transcript t00). The program is fine;
the runtime between it and the hardware
is missing or invisible. Work the list:

- Graphics card? Install its maker's
  driver stack (chapter 4's table), not
  just the display driver.
- CPU only? Install PoCL or the Intel
  CPU runtime — `sudo apt install pocl`
  on Ubuntu.
- Runtime installed but another copy
  shadows it? The lab's extracted-runtime
  variables (`OCL_ICD_VENDORS`) show the
  mechanism: the loader looks for `.icd`
  files to learn which runtimes exist.
  `hashcat -I` is the test that tells
  the truth after each change.

## "Token length exception … No hashes loaded"

The mode number does not match the
hash's format (transcript t07, kept
verbatim). Attacking an MD5 file with
`-m 100` (SHA-1) gives:

```
Token length exception near '8743b52063cd84097a65d1633f5c74f5'
(40 required but only 32 entered)
No hashes loaded.
```

Read the numbers: *required* is what the
mode expects, *entered* is what your
file has. Then confirm the real type
against `--example-hashes` (chapter 5)
and re-run. The same message appears for
truncated or pasted-with-spaces hashes —
check the file before blaming the mode.

## "Usage: hashcat [options]..." and exit, before anything runs

An argument is in a shape the parser
rejects — the lab met it asking for a
mask's keyspace with a hash file
attached (transcript t06). `--keyspace`
wants the attack description and nothing
else:

```
$ ./hashcat.bin -a 3 '?d?d?d?d' --keyspace
```

When usage prints, compare your line
against `./hashcat.bin --help` section
by section; the parser is strict about
which options take which partners.

## The run starts, then nothing seems to happen

Probably kernel compilation: the first
run of a mode on a device compiles its
kernel, and on a CPU runtime that can
take most of a minute with no progress
display. It happens once per mode per
device. If a *second* run of the same
mode also sits silent, watch the status
key (`s` in a terminal) and the
machine's own CPU meter before assuming
a hang — slow-hash modes at a few
hundred hashes per second look identical
to stuck for their first checkpoint.

## Everything reports as already recovered

`--show` answers from the potfile, and
attacks skip potfiled hashes entirely
(transcript t04). That is memory, not
malice. To see a run work fresh in
practice, use a hash you have not
cracked; to audit what the machine
remembers, read the potfile — and if
the potfile remembers things it should
not, on a machine that changed hands,
that is a finding, not a bug.

## The speed is absurdly low

Check three things before the hardware:

1. **Run size.** A 48-candidate run
   measures startup. Chapter 7's 536 H/s
   and chapter 10's 373.7 kH/s are the
   same machine; the difference is work
   shape (device-side amplification).
2. **Kernel kind.** Plain runs default
   to pure kernels; benchmarks use
   optimised ones (`-O`). Pure kernels
   accept longer passwords and run
   slower — the startup banner says so.
3. **Feed warnings.** "Hashcat is
   expecting at least 2048 base words"
   (transcript t06) means the candidate
   supply starves the device. Bigger
   lists, masks with more base, or rules
   as amplifier are the cures.

## It cracked in the example but not on my hash

The example pair proves the *pipeline*,
not the *password*. If your own hash of
the same mode does not fall, the likely
truths are the dull ones: the password
is not in your candidates (chapter 14),
the hash was copied with an extra
character or a trailing newline in the
password when it was created (chapter 6
warns about exactly this), or the hash
is of a slightly different construction
than the mode assumes — a salt position,
an encoding, a wrapper. Rebuild the
hash from a known plaintext with your
own hands, as chapter 6 does, and crack
that first: it separates "my attack is
wrong" from "my input is wrong" in one
run.
