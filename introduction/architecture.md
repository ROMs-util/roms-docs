# Architectural Overview

ROMs-util follows a tiered architecture inspired by the classic Linux `apt` and `dpkg` separation. This design ensures that the high-level management logic remains decoupled from the low-level installation engine.

---

## 🏗️ The Three-Tier Model

The ecosystem is divided into three distinct applications, each with a specialized responsibility:

### 1. The Manager (`roms`) - "The High-level Manager"
The high-level orchestrator. `roms` is what the user interacts with most.
*   **Responsibility:** Registry synchronization, dependency resolution, environment orchestration (PATH/Alternatives), UAC elevation routing, and **Truth-Verification** (proactive integrity checks on every startup).
*   **Key Logic:** 
    - **Atomic AVC**: Ensures transaction safety during installation.
    - **Truth-Verification Watchdog**: Guarantees a self-healing foundation by verifying the Standalone Engine against its manifest.
    - **Log-First Audit Strategy**: Logs removal intent *before* execution to ensure a reliable audit trail in `roms.log` even during race conditions.
*   **Data:** Manages the `index.json` (Registry) and `alternatives.json` (Shims).

### 2. The Engine (`rmspkg`) - "The Standalone Engine"
The low-level installer. It is designed to be a standalone, atomic engine.
*   **Responsibility:** Extraction, file copying, metadata registration, and uninstallation rollbacks.
*   **Key Logic:** Transactional IO. If a file copy fails mid-install, `rmspkg` wipes the destination to prevent pollution.
*   **Data:** Manages the `\.metadata` folder (the local database of installed artifacts).

### 3. The Builder (`package_builder`) - "The Gatekeeper"
The developer's tool for creating and distributing software.
*   **Responsibility:** Validating manifest schemas and bundling folders into compressed `.rms` packages.
*   **Key Logic:** Smart Exclusion. It automatically strips development artifacts (like `.git` or `.vscode`) to keep packages lean.

---

## ⚡ The .NET Rule: Native Performance

A core mandate of the ROMs-util architecture is the **.NET Rule**. 

In most Windows scripting environments, tools rely on PowerShell cmdlets (like `Expand-Archive` or `Get-FileHash`). These are often slow and behave differently across PowerShell versions.

**ROMs-util bypasses these cmdlets.** All core logic—Hashing, IO, Streams, and Compression—uses direct calls to native .NET namespaces:
*   `[System.IO.Compression.ZipFile]`
*   `[System.Security.Cryptography.SHA256]`
*   `[System.Net.Http.HttpClient]`

**Why it matters:**
1.  **Speed:** Near-native performance on large files.
2.  **Consistency:** Identical behavior on PowerShell 5.1 (Legacy) and PowerShell 7+ (Core).
3.  **Independence:** Zero reliance on external PowerShell modules.

---

## 📂 System Hierarchy

ROMs-util adheres to a strict, isolated directory structure rooted at `C:\roms`:

| Path | Name | Description |
| :--- | :--- | :--- |
| `C:\roms\bin` | **Bin** | Centralized launchers (Shims). Add this to your System PATH. |
| `C:\roms\.metadata` | **Registry** | Hidden database of installed package manifests and file lists. |
| `C:\roms\logs` | **Logs** | Transactional history of every install/uninstall action. |
| `C:\roms\cache` | **Cache** | Local copies of remote registry indexes. |
