# Writing criteria for this guide

Every "for Mere Mortals" guide is written to the same nine-point
standard, so a reader who trusts one can trust the rest. Here it
is, with the verification notes for this particular guide below.

1. **Fix it first.** Reader outcomes lead, not action items: each
   chapter opens by naming the outcome it delivers, shows a check
   the reader can run to confirm they got it, and a reasonable
   reader finishes able to state what was done, what it means,
   and what to try next.
2. **Plain language.** Short sentences, everyday vocabulary,
   technical terms defined on first use. No slang, and no
   idioms that only make sense in one country — the reader may
   be translating as they go.
3. **No dead ends.** A guide never tells the reader to do
   something it cannot show them how to verify. Every claim a
   reader relies on — an install, a command, an output — is
   something this guide demonstrates or clearly labels
   documented-only below.
4. **Live-verified where it matters.** Procedures are run for
   real in the guide's lab wherever the build machine allows it,
   and the transcripts kept in the repository are the evidence.
   What could not be run is listed honestly below, never
   presented as if it had been.
5. **General audiences.** Nothing in this guide — examples,
   fixtures, names, addresses — is modelled on any particular
   reader or author's real life, network, or organisations.
   The lab's passwords and wordlist were invented for the lab.
6. **Standalone.** A reader needs this guide, its lab, and
   nothing else. Where another guide would genuinely help, it
   is pointed to once, by name, and the chapter still completes
   without it.
7. **Honest about limits.** Chapter 14 names what the tool
   cannot do: no reversing, no finding a password outside the
   shape described, no beating length and randomness, speed
   figures that are local and temporary, and the potfile's
   double edge.
8. **The ethics rule is in the text, early.** For this guide
   the rule *is* chapter 2, before any procedure: your hashes,
   your hardware, your answers to know. Every fixture in the
   lab passes those tests — manufactured in the lab from
   invented passwords — and the text says plainly that breach
   material is other people's credentials and never practice
   fodder.
9. **Builds clean.** The site builds with MkDocs in strict mode
   with no warnings, and a fresh reader following the chapters
   in order reaches the stated outcome of each.

## Verification notes for this guide

The build machine is a 2-CPU, ~8 GB Ubuntu 24.04 sandbox,
shared with other work while the lab ran (speeds are therefore
ranges, and the guide says so). Hashcat **8.0.0** was installed
from the official release archive `hashcat-8.0.0.7z`
(SHA-256 `9fa86e1bfd5dd898d3c83aa38a67b08db1ba5d2f5252dffa2af44a8c62585f49`)
and run unmodified. The machine has **no graphics card and had
no OpenCL runtime at all**; the CPU device used throughout is
PoCL 5.0 (Ubuntu noble packages, LLVM 16), because hashcat's
own site names PoCL as the CPU route. The sandbox's package
database belongs to other tenants, so the lab downloaded the
same packages with a private apt state and unpacked them
(`dpkg -x`, unmodified) into a scratch tree, selecting them
with `LD_LIBRARY_PATH` and `OCL_ICD_VENDORS`. On a normal
machine the identical step is `sudo apt install pocl`, and
chapter 4 says so. All transcripts live in
`hashcat-lab/transcripts/`; the wordlists, rule file and
practice hashes live in `hashcat-lab/`.

Run for real, with transcripts as evidence:

- The official download, its checksum, `hashcat --version`
  (v8.0.0), and `hashcat -I` **failing** before any runtime
  existed — `clGetPlatformIDs(): CL_PLATFORM_NOT_FOUND_KHR`,
  exit 255, quoted verbatim (t00).
- PoCL stood up from the extracted packages; `hashcat -I`
  then lists one CPU device ("AMD EPYC 9D25", 2 processors,
  OpenCL 3.0 PoCL 5.0) (t01).
- `--example-hashes` for modes 0 and 7400, read field by
  field; practice hashes manufactured with `md5sum` and
  `openssl passwd -5` (fixed salt `labsalt42`, so the
  transcripts repeat exactly); a 48-entry invented wordlist
  (t02).
- Dictionary attack on two MD5 hashes in one file:
  **2/2 recovered** — the `--example-hashes` sample
  (`hashcat`) and the lab's own (`bluebird7`) (t03).
- Proof three ways: `--show`, the potfile read directly
  (the release build keeps it in the program folder —
  chapter 3 was corrected to match), the answer re-hashed
  with `md5sum` to the identical fingerprint, and the
  verbatim potfile-lookup notice on a repeat run (t04).
- Dictionary attack on a salted sha256crypt hash:
  **recovered** (`trumpet99`) at a measured 40 H/s in-run,
  `Iterations: 5000` in the status block (t05).
- Mask attack `?d?d?d?d` on an MD5 of a four-digit PIN:
  **10,000/10,000 progress, recovered** (`4071`), including
  the `--keyspace` usage failure and its corrected form,
  with the base/amplifier explanation the numbers forced
  (t06, startup lines trimmed and marked as trimmed).
- A wrong-mode run kept verbatim: MD5 hashes under `-m 100`,
  "Token length exception … 40 required but only 32
  entered", "No hashes loaded", exit 244 (t07).
- Rules attack: an eight-rule lab file over a 47-word list
  that deliberately lacks the target's dictionary form —
  376 candidates, **recovered** (`Bluebird7` from
  `bluebird` via `c$7`), 373.7 kH/s (t08, trimmed as marked).
- Benchmarks on this machine, run twice: MD5 113.7 and
  124.2 MH/s, sha256crypt 499 and 554 H/s — the guide quotes
  the second pair with the first noted, and computes its
  ~224,000 factor from matching pairs only. The final
  potfile, five invented recoveries, closes the record (t09).
- Extracting field 2 of an invented shadow-style line with
  `cut -d: -f2` (t10).

Documented only — described from the project's documentation,
not run, and the text does not claim otherwise:

- Everything on a graphics card: CUDA, ROCm and NEO runtimes,
  GPU rates, multi-device runs. Chapter 11's benchmark is the
  reader's instrument for their own hardware.
- bcrypt (mode 3200) and Argon2 cracking: characterised
  (cost exponent, memory) but not run; sha256crypt is the
  slow hash the lab measured.
- Attack modes 1 (combinator) and 6/7 (hybrid): explained
  from the model and small arithmetic, not executed.
- The shipped rule files (`best64.rule`, `dive.rule`):
  named and sized, not run; the lab's own eight-line rule
  file is the executed evidence.
- Hash extraction from real files (`*2john` helpers), Wi-Fi
  handshake modes (22000), distributed cracking, Brain, and
  sessions resumed across machines: pointed to, not taught
  by doing. Hash *capture* of any kind is out of scope.
- The neighbour tools of chapter 13 (John the Ripper,
  hashid, online lookup services): characterised, not run
  or benchmarked.

Fixtures are this guide's own and deliberately generic:
five invented passwords (`hashcat` is hashcat's own published
example plaintext), a 48-word invented wordlist, one fixed
lab salt, and an invented shadow line for an invented user.
No real person's password, and no hash of one, appears
anywhere in this repository; the potfile in the transcripts
contains only the lab's own recoveries.
