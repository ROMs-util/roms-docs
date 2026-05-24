# Welcome to ROMs-util

**Industrial Strength Windows Package Management.**

ROMs-util is a high-performance, zero-dependency ecosystem designed to bring advanced orchestration patterns to the Windows environment. Powered by native .NET performance, it ensures your system utilities remain portable, immutable, and easy to manage.

---

## 🚀 The ROMs Philosophy

Most Windows managers rely on heavy external frameworks or inconsistent PowerShell scripts. ROMs-util is built differently:

*   **Zero External Dependencies:** We bypass slow cmdlets in favor of native .NET namespaces (`[System.IO]`, `[System.Security.Cryptography]`).
*   **Safety by Design:** Our flagship **Atomic AVC** model ensures your machine is never polluted by partial or failed installations.
*   **Ecosystem Harmony:** The **Alternatives** system resolves command name collisions, allowing multiple versions of the same tool to coexist peacefully.

---

## 🗺️ Portal Navigation

Whether you are here to install a tool or build your own, we have you covered:

### For Users
*   [**Quick Start**](introduction/quick-start.md): Set up the ecosystem in under 60 seconds.
*   [**CLI Reference**](user-guide/cli-reference.md): Master the `roms` command-line interface.
*   [**Alternatives**](user-guide/alternatives.md): Learn how to switch between different tool providers.

### For Developers
*   [**Architecture**](introduction/architecture.md): Understand the tiered management model.
*   [**Building Packages**](developer-guide/creating-packages.md): Turn your tools into `.rms` packages using the `builder`.
*   [**Manifest Spec**](developer-guide/manifest-spec.md): Technical details of the `roms_package.json` format.

---

## 🛡️ The Industrial Strength Guarantee
Every tool in this ecosystem is audited for performance and version-independence. We don't just script; we engineer.
