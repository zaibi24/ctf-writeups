# picoCTF — ReadmyCert Challenge Writeup

**Challenge Name:** ReadmyCert  
**Category:** Cryptography  
**Flag:** `picoCTF{read_mycert_373b4ab0}`

---

## Description

We were given a CSR (Certificate Signing Request) file and asked to find the flag hidden inside it.

---

## What is a CSR?

A **Certificate Signing Request (CSR)** is a Base64-encoded public file submitted to a Certificate Authority (CA) to apply for a digital certificate. It contains:

- **Subject info** (Common Name, Organization, Country)
- **Public Key**
- **Signature**

> ⚠️ A CSR does NOT contain any private key or secret — it is entirely public data. So there is nothing to "decrypt", just decode!

---

## Solution

### Step 1 — Recognize the format

The file was Base64-encoded DER data — a standard CSR format (PEM):

```
MIICpzCCAY8CAQAwPDEmMCQGA1UEAwwdcGljb0NURntyZWFkX215Y2VydF8zNzNi
NGFiMH0xEjAQBgNVBCkMCWN0ZlBsYXllcjCCASIwDQYJKoZIhvcNAQEBBQADggEP
...
```

### Step 2 — Decode with OpenSSL

```bash
openssl req -in file.csr -noout -text
```

Or decode directly from Base64:

```bash
echo "<base64_data>" | base64 -d | openssl req -inform DER -noout -text
```

### Step 3 — Read the output

```
Certificate Request:
    Data:
        Subject: CN = picoCTF{read_mycert_373b4ab0}, name = ctfPlayer
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                Public-Key: (2048 bit)
        Attributes:
            X509v3 Extended Key Usage:
                TLS Web Client Authentication
```

The flag was hidden right in the **CN (Common Name)** field of the Subject!

---

## Flag

```
picoCTF{read_mycert_373b4ab0}
```

---

## Key Takeaway

CTF challenges sometimes hide flags in unexpected places — like certificate fields. Always inspect the metadata and fields of any file format you encounter. A CSR is just structured public data — readable with standard tools like `openssl`.
