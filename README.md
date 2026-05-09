# bash-vsv (portable fork)

A lightweight Bash frontend for managing runit services via `sv`.

This fork is based on `bahamas10/bash-vsv`, which has been officially deprecated.

It adds compatibility fixes for non-Void Linux runit layouts, particularly AntiX and other systems that do not use `/var/service`.

---

## Status

- Original project: deprecated
- This fork: maintained compatibility version
- License: MIT (same as upstream)

---

## Why this fork exists

The original script assumes the Void Linux runit service directory:

```
/var/service
```

However, many runit-based systems use different layouts, for example:

- AntiX Linux: `/etc/runit/runsvdir/default`
- Some systems: `/run/runit/service`
- Custom setups: user-defined `SVDIR`

This mismatch causes the tool to fail with:

```
FATAL: failed to enter dir: /var/service
```

This fork resolves that by adding runtime detection of the correct runit service directory.

---

## Changes from upstream

### 1. Portable service directory detection

The tool now detects `SVDIR` in the following order:

1. `$SVDIR` (if explicitly set by user)
2. `/var/service` (Void Linux default)
3. `/etc/runit/runsvdir/default` (AntiX default)
4. `/run/runit/service` (generic fallback)

If none exist, the tool exits with an error.

---

### 2. Improved portability across runit systems

No assumptions are made about a single distro layout.

Works on:

- AntiX Linux
- Void Linux
- Devuan (runit setups)
- Generic runit installations

---

## Installation

### Manual install (recommended)

```bash
git clone https://github.com/5lineconfigs/bash-vsv
cd bash-vsv
sudo install -m 755 vsv /usr/local/bin/vsv
```

---

## Usage

### Basic

```bash
vsv
```

### Explicit service directory

```bash
SVDIR=/etc/runit/runsvdir/default vsv
```

### Root usage

```bash
sudo vsv
```

---

## Examples

### List services

```bash
vsv
```

### Check status

```bash
vsv status ssh
```

### Restart a service

```bash
sudo vsv restart ssh
```

### Stop a service

```bash
sudo vsv stop cups
```

---

## Requirements

- Bash
- runit (`sv`, `runsvdir`)
- standard core utilities (`ps`, `cd`, etc.)

---

## Notes

- This is a lightweight wrapper around `sv`
- It does not replace runit itself
- It assumes a standard runit service tree exists on the system
- Root privileges are required for managing system services

---

## License

MIT License — same as upstream `bahamas10/bash-vsv`.

Original author retains copyright of original work.
Fork modifications © 2026 5lineconfigs.

---

## Repository

https://github.com/5lineconfigs/bash-vsv
```
