# 🕵️ Impostor - CTF Writeup

**Challenge Name:** Impostor  
**Points:** 100  
**Author:** grdnero  

---

## 📌 Description

> Someone has been wearing another person's face, and the only name left on the mask is an address:  
> **h111larym41tl14nd@gmail.com**  
>
> A quiet impostor appeared in the mirrors of a half-forgotten identity.  
> The name looked normal, but the person behind it did not.  
> The inbox is not the target. The address itself is the disguise.

---

## 🧠 Solution Approach

The challenge hints that the **email itself is not meant for inbox access**, but rather serves as a **clue/identity**.

### 🔍 Step 1: Analyze the Email
The given email:

h111larym41tl14nd@gmail.com

It looks obfuscated (leet-style), resembling a real name:

hillary maitland (approx)

---

### 🌐 Step 2: Search the Email

Instead of logging into the inbox, I searched this email on the internet using tools like:

- Google
- Username lookup sites

This led to a **GitHub profile** linked with the same identity.

---

### 🐙 Step 3: Investigate GitHub Profile

On visiting the GitHub profile, I checked:

- Bio section
- Repositories
- Commits

In the **bio**, I found a suspicious encoded string:

Q1RGQUN7ZmEyNDNlOTBlYjRhNzIwZDk1NmQ5ZGNkNmU3ZTk0ZTc2YzI3YmZjNmRlOTdhMTc0ZDkwYmIxNWRiY2Q4NWM4ZX0=

---

### 🔐 Step 4: Decode Base64

The string looked like **Base64 encoding**.

Decoding it gave:

CTFAC{fa243e90eb4a720d956d9dcd6e7e94e76c27bfc6de97a174d90bb15dbcd85c8e}

---

## 🏁 Final Flag

CTFAC{fa243e90eb4a720d956d9dcd6e7e94e76c27bfc6de97a174d90bb15dbcd85c8e}

---

## 💡 Key Takeaways

- Not all email-based challenges require inbox access
- OSINT (Open Source Intelligence) is crucial
- GitHub profiles often hide flags in:
  - Bio
  - Commits
  - README files
- Always check for encoding (Base64 is common)

---

## 🧩 Tools Used

- Google Search
- Username lookup websites
- Base64 Decoder

---

**Done ✅**
