# Glossary

**Attack mode** — where the password
candidates come from in a run: a wordlist
(straight), two lists glued together
(combinator), a pattern (mask), or a list
plus a pattern (hybrid). Chosen with
`-a`.

**Backend** — the software layer hashcat
uses to reach compute hardware: OpenCL,
CUDA, HIP, or Metal. A machine can offer
several; `-I` shows what hashcat found.

**Candidate** — one guessed password,
waiting to be hashed and compared. Runs
are measured in candidates per second as
often as in hashes per second; for
salted hashes the two numbers differ.

**Crack** — to recover the plaintext of
a hash by finding an input that
reproduces it. Despite the word, nothing
is broken open: the answer is guessed,
then proven by the matching fingerprint.

**Device** — one piece of hardware a
backend exposes: a graphics card, or a
processor seen through a CPU OpenCL
runtime, as in this guide's lab.

**Digest** — another word for a hash
value, especially the output of a hash
function applied once.

**Hash** — the fixed-size output of a
hash function; a fingerprint of the
input. Also, loosely, the function
itself.

**Hash function** — a one-way scrambling
algorithm (MD5, SHA-256, bcrypt's
construction) that turns any input into
a fixed-size output, with no key and no
way back except guessing.

**Hash mode** — hashcat's numbered name
for a hash type: `-m 0` is MD5, `-m 7400`
is sha256crypt, `-m 3200` is bcrypt. The
number is a promise about the format of
every line in the hash file.

**Kernel** — the small per-mode program
that computes a hash on a device.
Kernels are compiled on first use and
cached on disk for later runs.

**Keyspace** — the total number of
candidates a run will try: wordlist
length times rules, or the size of a
mask's pattern space. `--keyspace`
prints it without running anything.

**Mask** — a pattern for generating
candidates from character sets: `?d` is
a digit, `?l` a lowercase letter, `?a`
any printable character, so `?d?d?d?d`
is every four-digit sequence.

**Potfile** — hashcat's memory: the file
where recovered pairs of hash and
plaintext are appended. `--show` reads
it back. A file of working passwords —
handle it like one.

**Rules** — a file of small mutations
(capitalise, append a digit, swap a
letter) applied to every wordlist
candidate in a straight attack, chosen
with `-r`.

**Salt** — a random value stored with a
hash and mixed into it, so identical
passwords hash differently and each hash
must be attacked separately.

**Session** — one named run of hashcat,
with its progress checkpoints; an
interrupted session can be resumed with
`--restore`.

**Slow hash** — a hash construction
(bcrypt, sha256crypt, Argon2) built to
cost time and memory per guess, so that
billions of guesses become years. The
defence chapter 11 measures.

**Straight attack** — attack mode 0:
candidates come from a wordlist, as
written (plus rules, if any).

**Wordlist** — a text file of candidate
passwords, one per line. Order matters:
the likely ones should come early.
