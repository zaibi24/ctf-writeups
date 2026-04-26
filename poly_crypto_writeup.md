# Poly-Crypto — Writeup

## Challenge Summary
A corrupted export provided three artifacts:

- `artifact.png`
- `output.txt`
- `solve.py` (redacted)

The goal was to recover the missing RSA private key material and decrypt the ciphertext.

---

## Files Provided

### output.txt
Contained standard RSA public values:

```text
n = ...
e = 65537
c = ...
```

### solve.py (partial)

```python
from hashlib import sha512
from sympy import nextprime

def derive():
    a = get_part_a()
    b = get_part_b()

    p = nextprime(int.from_bytes(sha512(b"P" + a).digest(), "big"))
    q = nextprime(int.from_bytes(sha512(b"Q" + b).digest(), "big"))
```

This shows the RSA primes were deterministically derived from two hidden byte strings:

- `a`
- `b`

So instead of factoring `n`, we only needed to recover those hidden inputs.

---

## Key Insight: “Poly-Crypto”
The title strongly hinted at **multiple formats / multiple layers**.

The PNG was not just an image—it was a **polyglot file** containing hidden embedded data.

---

## Step 1 — Analyze artifact.png
Use common forensic tools:

```bash
binwalk artifact.png
strings artifact.png
exiftool artifact.png
```

This revealed extra embedded file signatures inside the PNG:

- PDF
- MP4

---

## Step 2 — Extract Embedded Files
Using `binwalk -e` or manual carving, extract the hidden payloads.

```bash
binwalk -e artifact.png
```

Recovered:

- embedded PDF
- embedded MP4

---

## Step 3 — Recover Hidden Parts

### From PDF
Text/image content revealed two hexadecimal fragments:

```text
9f2c6b7a1d84e3c
0f5a927d6bce41308
```

Combined:

```text
9f2c6b7a1d84e3c0f5a927d6bce41308
```

This was used as one hidden input.

### From MP4
The final frame contained:

```text
41E8D0F6A3B95C72DD19A4F08BE6C5AA
```

This was the second hidden input.

---

## Step 4 — Rebuild RSA Primes

```python
from hashlib import sha512
from sympy import nextprime

A = bytes.fromhex("9f2c6b7a1d84e3c0f5a927d6bce41308")
B = bytes.fromhex("41E8D0F6A3B95C72DD19A4F08BE6C5AA")

p = nextprime(int.from_bytes(sha512(b"P" + A).digest(), "big"))
q = nextprime(int.from_bytes(sha512(b"Q" + B).digest(), "big"))
```

Then verify:

```python
assert p * q == n
```

---

## Step 5 — Decrypt Ciphertext

```python
from Crypto.Util.number import long_to_bytes

phi = (p - 1) * (q - 1)
d = pow(e, -1, phi)
m = pow(c, d, n)
print(long_to_bytes(m))
```

---

## Flag

```text
CTFAC{a8de7eec2b0d529d0c234098e6988a9133110a89af5080d0c465868a5794bbe2}
```

---

## Final Notes
This challenge combined:

- file forensics
- polyglot containers
- deterministic prime generation
- RSA reconstruction

A nice reminder that crypto challenges often hide the weakness **outside** the math.

