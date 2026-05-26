# Quick Start Guide

Welcome to the ROMs-util ecosystem. This tutorial will guide you through setting up the environment and installing your first package in under 60 seconds.

---

## 🛠️ Step 1: Prerequisites

Before you begin, ensure your machine meets the minimum requirements:
*   **OS:** Windows 10 or 11 (x64).
*   **Shell:** PowerShell 5.1 (Built-in) or PowerShell 7+ (Recommended).
*   **Access:** Administrator privileges (required for system-wide installs).

---

## 🚀 Step 2: Initialize the Ecosystem

ROMs-util is designed to be zero-setup. To initialize the ecosystem root, simply create the directory:

```powershell
# Create the ecosystem root
New-Item -ItemType Directory -Path "C:\roms\bin" -Force
```

> **Important:** Add `C:\roms\bin` to your **System PATH**. This ensures that any tool you install via ROMs is immediately available from any terminal.

---

## 📦 Step 3: Install Your First Tool

The package manager (`roms`) is the heart of the ecosystem. To install a package from the official registry, use the `install` command.

Let's install the **Autofirewall** utility as an example:

```powershell
# Navigate to the manager directory (or use the shim if already installed)
.\roms.bat install autofirewall -y
```

### What just happened?
1.  **Map:** `roms` identified the latest version of Autofirewall.
2.  **Acquire:** The engine was self-healed/bootstrapped (if missing) and verified by the **Truth-Verification Watchdog**. The `.rms` package was staged and verified for integrity.
3.  **Commit:** The tool was extracted to `C:\roms\autofirewall` and a launcher was created in `C:\roms\bin`.

---

## 🔍 Step 4: Verify the Installation

You can now run the newly installed tool from any directory:

```powershell
autofirewall --help
```

To see a list of everything installed on your machine:

```powershell
.\roms.bat list
```

---

## 🎉 Next Steps

*   Explore the [**Registry**](../user-guide/cli-reference.md) to find more tools.
*   Learn how to manage multiple tool versions with [**Alternatives**](../user-guide/alternatives.md).
*   Ready to build your own? Head over to the [**Developer Guide**](../developer-guide/creating-packages.md).
