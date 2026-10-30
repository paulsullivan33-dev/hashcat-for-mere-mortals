# Installing Hashcat

By the end of this chapter, hashcat runs
on your machine and can see at least one
compute device. That second half is the
real work: hashcat is a small program,
but it is useless until a **backend**
hands it some hardware.

**Check you can run:** `./hashcat.bin -I`
lists a device — a graphics card, or your
processor. If it instead prints "No
OpenCL, HIP or CUDA compatible platform
found", you are where this guide's lab
started, and this chapter is the fix
(transcripts t00 and t01).

## The program itself

Hashcat is free and open source (MIT
licence). The official release is a
single archive from
`https://hashcat.net/hashcat/` — for this
guide, `hashcat-8.0.0.7z`, about 20 MB.
Unpack it anywhere; there is no
installer. Inside, the Linux program is
`hashcat.bin`, and it expects to be run
from its own folder, where its kernels,
modules and potfile live.

```
$ curl -sSL -O https://hashcat.net/files/hashcat-8.0.0.7z
$ sha256sum hashcat-8.0.0.7z
9fa86e1bfd5dd898d3c83aa38a67b08db1ba5d2f5252dffa2af44a8c62585f49
$ 7z x hashcat-8.0.0.7z
$ cd hashcat-8.0.0
$ ./hashcat.bin --version
v8.0.0
```

Your distribution may also package
hashcat. A packaged copy is fine and
lands on your `PATH` as plain `hashcat`;
it will typically be an older version
than the release archive, and its potfile
lives under `~/.local/share/hashcat/`
instead of in the program folder. This
guide uses the release archive, so its
commands say `./hashcat.bin`.

## The runtime: the part that is not hashcat

Hashcat computes on hardware through a
backend — OpenCL, CUDA, HIP or Metal —
and the backend's runtime must come from
the hardware's maker:

- **NVIDIA graphics cards:** the NVIDIA
  driver plus CUDA toolkit.
- **AMD graphics cards on Linux:** ROCm.
- **Intel graphics:** the Intel Graphics
  Compute Runtime (NEO).
- **Any processor (CPU):** the Intel CPU
  Runtime for OpenCL, or **PoCL**, the
  portable open-source one.
- **Apple machines:** nothing to install;
  macOS ships Metal and OpenCL.

The lab machine has no graphics card, so
it took the last-but-one road. On Ubuntu,
PoCL is one package:

```
$ sudo apt install pocl
```

The lab machine's package database was
shared with other tenants, so the lab
fetched the same Ubuntu packages and
unpacked them unmodified into a scratch
folder, pointing the OpenCL loader at
them with two environment variables.
Same code, same version — PoCL 5.0,
LLVM 16 — only the plumbing differed,
and transcript t01 shows it. Do not copy
the lab's plumbing onto a normal machine;
`apt install pocl` is the whole job
there.

## Success looks like this

```
$ ./hashcat.bin -I
OpenCL Platform ID #1
  Vendor..: The pocl project
  Name....: Portable Computing Language
  Version.: OpenCL 3.0 PoCL 5.0 ...
  Backend Device ID #01
    Type...........: CPU
    Name...........: cpu-...-AMD EPYC ... Processor
    Processor(s)...: 2
```

One device is enough for everything in
this guide. A CPU will not set speed
records — chapter 11 measures exactly
what it does manage — but every attack
mode works, and the lessons transfer
unchanged to a graphics card, where the
same commands run faster by orders of
magnitude.

A first run of any mode pauses while the
device compiles that mode's kernel. The
compiled kernel is cached, so the pause
happens once per mode per device. On the
lab's small CPU the whole of chapter 7
took under ten seconds, compile included;
on a fresh graphics-card driver install
it can take a minute. Patience, once,
is normal.
