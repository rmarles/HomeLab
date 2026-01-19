# GitHub Default License Options – A Practical Decision Guide

This document summarizes the **default/open‑source license options GitHub commonly suggests** when creating a repository, explains **key differences**, and provides a **decision matrix** to help you choose the right license based on your goals.

This is written to be practical rather than academic — something you can drop directly into a repo as `README.md` or keep as a reference.

---

## Why Licenses Matter (Short Version)

A license answers three critical questions for anyone using your code:

1. **Can I use this?**
2. **Can I modify it?**
3. **Can I redistribute it (commercially or not)?**

Without a license, the default answer to all three is **“no.”**

---

## Common GitHub License Options

GitHub typically highlights these licenses during repo creation:

- MIT License
- Apache License 2.0
- GNU GPL v3.0
- BSD 2‑Clause / 3‑Clause
- GNU LGPL v3.0
- Mozilla Public License 2.0 (MPL)
- Unlicense

---

## High‑Level Comparison

| License | Permissive | Requires Sharing Changes | Commercial Use | Patent Protection | Common Use Case |
|------|------------|--------------------------|----------------|-------------------|-----------------|
| MIT | ✅ Yes | ❌ No | ✅ Yes | ❌ No | Libraries, tools, examples |
| Apache 2.0 | ✅ Yes | ❌ No | ✅ Yes | ✅ Yes | Enterprise / corporate projects |
| BSD | ✅ Yes | ❌ No | ✅ Yes | ❌ No | Academic / infrastructure code |
| GPL v3 | ❌ No | ✅ Yes (strong copyleft) | ✅ Yes | ✅ Yes | Ideological open source |
| LGPL v3 | ⚠️ Partial | ⚠️ Libraries only | ✅ Yes | ✅ Yes | Shared libraries |
| MPL 2.0 | ⚠️ Partial | ⚠️ File‑level | ✅ Yes | ✅ Yes | Mixed proprietary + OSS |
| Unlicense | ✅ Yes | ❌ No | ✅ Yes | ❌ No | Public domain dedication |

---

## License‑by‑License Breakdown

### MIT License
**TL;DR:** *Do whatever you want, just don’t sue me.*

**You should choose MIT if:**
- You want **maximum adoption**
- You don’t care if someone uses your code commercially
- You want **minimal legal overhead**

**Pros**
- Extremely simple
- Widely understood
- Compatible with almost everything

**Cons**
- No patent protection
- No obligation for improvements to be shared back

---

### Apache License 2.0
**TL;DR:** *MIT, but lawyer‑friendly.*

**You should choose Apache 2.0 if:**
- You want permissive licensing **with patent protection**
- Your project may be used by enterprises
- You want explicit contributor protections

**Pros**
- Patent grant included
- Clear contributor rights
- Business‑safe

**Cons**
- Slightly more complex than MIT

---

### BSD 2‑Clause / 3‑Clause
**TL;DR:** *MIT with academic roots.*

**You should choose BSD if:**
- You’re publishing infrastructure or research‑style code
- You want permissive use with minimal requirements

**Pros**
- Simple
- Permissive

**Cons**
- Less common outside academia
- No patent protections

---

### GNU GPL v3.0
**TL;DR:** *If you use it, you must share it.*

**You should choose GPL if:**
- You want **all derivatives to remain open source**
- You are philosophically committed to software freedom

**Pros**
- Guarantees openness
- Strong legal protections
- Patent protections included

**Cons**
- Incompatible with many commercial products
- Reduces adoption in corporate environments

---

### GNU LGPL v3.0
**TL;DR:** *GPL, but friendlier to libraries.*

**You should choose LGPL if:**
- You’re publishing a **library**
- You want improvements to the library shared back
- You still want commercial usage allowed

**Pros**
- Encourages contribution
- Allows proprietary linking

**Cons**
- More complex to understand
- Still avoided by some businesses

---

### Mozilla Public License 2.0 (MPL)
**TL;DR:** *Open core, closed product.*

**You should choose MPL if:**
- You want **modifications shared**, but only to specific files
- You expect a mix of proprietary and open code

**Pros**
- Balanced approach
- File‑level copyleft
- Patent protections

**Cons**
- Less common
- Slightly confusing to newcomers

---

### Unlicense
**TL;DR:** *I don’t care. Do anything.*

**You should choose Unlicense if:**
- You want to dedicate your work to the **public domain**
- You truly want zero restrictions

**Pros**
- Maximum freedom
- No obligations

**Cons**
- Not recognized equally in all jurisdictions
- No protections at all

---

## Decision Matrix (Start Here)

Answer these questions in order:

1. **Do you want improvements to be shared back?**
   - Yes → GPL / LGPL / MPL
   - No → MIT / Apache / BSD

2. **Do you care about patent protection?**
   - Yes → Apache 2.0 / GPL / LGPL / MPL
   - No → MIT / BSD

3. **Is commercial adoption important?**
   - Yes → MIT / Apache / BSD / MPL
   - No → GPL

4. **Are you building a library?**
   - Yes → MIT / Apache / LGPL
   - No → Any

---

## Practical Recommendations

**Default choice (90% of projects):**
> **MIT License**

**Enterprise‑friendly default:**
> **Apache License 2.0**

**Ideological open source:**
> **GPL v3.0**

**Balanced open/proprietary mix:**
> **MPL 2.0**

---

## Final Notes

- You can **change licenses later**, but it’s painful once contributors exist.
- Pick the **simplest license that achieves your goals**.
- When in doubt: **MIT or Apache 2.0**.

---

*This document is informational, not legal advice.*

