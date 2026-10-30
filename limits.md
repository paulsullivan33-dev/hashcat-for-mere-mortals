# Limits

What hashcat cannot do, stated as
plainly as what it can — because a tool
whose limits are understood is safe to
use, and a tool believed to be magic is
not.

**It cannot reverse a hash.** No mode,
no option, no future version changes
this: the mathematics has no backwards.
Every recovery in this guide is a guess
that matched. Where no guess matches,
there is no recovery, full stop.

**It cannot find a password outside the
shape you describe.** A dictionary finds
only listed words and their ruled
mutations; a mask finds only its own
pattern. A correct password in a shape
you did not ask about is indistinguishable
from a strong password — both simply
never appear among the candidates.

**It cannot beat a long random
password.** The arithmetic is the whole
story. Sixteen characters drawn at
random from `?a`'s 95 make about 10³¹
candidates; at this lab machine's
124 million MD5 guesses per second that
is roughly a trillion trillion years,
and the fastest graphics-card rigs on
earth only shave exponents, they do not
change the conclusion. Length and
randomness are not "harder". They are a
different country.

**Slow hashes cut the other way, but
not infinitely.** Chapter 11's factor
of ~224,000 turns hours into years for
big searches — and does nothing at all
when the password is in the first
thousand candidates of a good list, as
`trumpet99` demonstrated by falling at
40 H/s. Slowness multiplies search cost;
it does not remove the password from
the search space.

**Speed figures are local and
temporary.** Every rate in this guide
was measured on a shared two-processor
sandbox, through a CPU OpenCL runtime,
and moved by ten percent between runs.
Your hardware, your driver, your kernel
cache will give different numbers.
Benchmark (`-b`) before you plan; the
guide's figures exist to teach the
ratios, not to promise rates.

**Kernel compilation is a first-run
tax, and runtimes are a dependency.**
A mode costs a compile the first time
it meets a device, and a broken or
missing OpenCL/CUDA runtime means the
fastest hashcat in the world reports
zero devices (chapter 4's starting
point). The tool is only ever as healthy
as the driver stack under it.

**The potfile cuts both ways.** It makes
audits cumulative and re-runs free —
and it is a growing plaintext list of
every password you have ever recovered
on that machine. Chapter 2's handling
rule is not decoration: on a real audit,
the potfile is the most sensitive file
in the room.

**Finally, the limit that is not
technical.** Hashcat will happily spend
a week on hashes you had no right to
test. The tool has no opinion about
chapter 2. You are the opinion.
