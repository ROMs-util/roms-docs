# 🛡️ Atomic AVC Model

The **Atomic Acquire-Verify-Commit (AVC)** model is the flagship safety feature of the ROMs-util ecosystem. It is designed to solve the problem of "Polluted Failures"—a common issue in script-based managers where a failed installation leaves half-installed dependencies or orphan files on your system.

---

## 🛑 The Problem: Polluted Failures

In a traditional manager, if you install a package that requires five dependencies:
1.  It installs Dependency A.
2.  It installs Dependency B.
3.  Dependency C fails to download.
4.  The process stops.

**The result:** Your machine now has Dependency A and B installed, even though the primary tool you wanted is missing. This "garbage" accumulates over time, making the system unpredictable.

---

## ✅ The Solution: The AVC Lifecycle

The ROMs-util manager (`roms`) prevents this by using a 3-phase atomic workflow. **No system modifications are made until success is guaranteed.**

### Phase 1: Mapping (Pre-Check)
The manager recursively walks the dependency tree. It identifies every package that needs to be present.
*   *Action:* Registry Lookup.
*   *Constraint:* If any link in the chain is missing from the registry, the process aborts immediately. **Disk touch: ZERO.**

### Phase 2: Acquisition & Verification (Staging)
The manager downloads or copies all required `.rms` files into a isolated staging area (`C:\roms\temp\staging`).
*   *Action:* Staging & Integrity Check.
*   *Constraint:* Every file is hashed using SHA256. If a single download is corrupted or fails, the staging area is wiped. **Disk touch: TEMP ONLY.**

### Phase 3: Commitment (Installation)
Only if Phase 1 and Phase 2 are 100% successful does the manager proceed to installation.
*   *Action:* Serial call to the `rmspkg` engine.
*   *Result:* All dependencies and the primary package are installed together.

---

## 📊 Flow Comparison

### Traditional Logic (Unsafe)
`Resolve -> Install A -> Resolve -> Install B -> FAIL (System Polluted)`

### Atomic AVC Logic (Safe)
`Resolve All -> Stage All -> Verify All -> Install All (System Clean on Fail)`

---

## 🛡️ Industrial Strength Guarantee
By implementing AVC, ROMs-util ensures that your `C:\roms` directory remains a pristine environment. Every successful `roms install` is a complete transaction, and every failure is a non-event for your system files.
