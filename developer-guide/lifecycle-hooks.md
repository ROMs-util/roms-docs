# Lifecycle Hooks

Lifecycle hooks allow developers to execute custom PowerShell scripts at specific points during the installation or uninstallation process. ROMs-util uses a **Hybrid Standard** to ensure maximum flexibility and ease of use.

---

## 🏗️ The Hybrid Model

ROMs-util supports two ways to define hooks:

1.  **Explicit (Manifest-First):** Define custom filenames in the `roms_package.json` file.
2.  **Implicit (Fallback-Second):** Use industry-standard filenames in the package root.

### Why Hybrid?
The explicit method allows you to name your scripts anything (e.g., `init-db.ps1`), while the implicit method provides a "zero-config" experience for standard setup scripts.

---

## 📦 Supported Hook Events

| Event | Manifest Key | Standard Filename (Fallback) | Timing |
| :--- | :--- | :--- | :--- |
| **Pre-Install** | `preInstall` | `pre-install.ps1` | Runs **before** files are extracted. |
| **Post-Install** | `postInstall` | `post-install.ps1` | Runs **after** extraction and metadata registration. |
| **Pre-Uninstall** | `preUninstall` | `pre-uninstall.ps1` | Runs **before** files or metadata are deleted. |
| **Post-Uninstall** | `postUninstall` | `post-uninstall.ps1` | Runs **after** the package directory is removed. |

---

## 🛠️ Implementation Guide

### 1. Defining Hooks in the Manifest
To use custom filenames, add the `hooks` object to your `roms_package.json`:

```json
{
    "name": "my-app",
    "version": "1.0.0",
    "hooks": {
        "postInstall": "setup-env.ps1",
        "preUninstall": "cleanup-service.ps1"
    }
}
```

### 2. Standard Fallback Naming
If you don't want to modify your manifest, simply name your scripts following the **Clean Kebab-Case** standard in your package root:
*   `post-install.ps1`
*   `pre-uninstall.ps1`

> **Note:** The legacy `rms_install.ps1` and `rms_uninstall.ps1` names are deprecated and will be removed in a future release. Please transition to the kebab-case standard.

---

## 🛡️ Requirements

To ensure ecosystem stability, all hooks must adhere to these rules:

### 1. The Exit Code Mandate
The engine (`rmspkg`) monitors the `$LASTEXITCODE` of every hook. 
*   If an **Installation Hook** (Pre/Post) returns a non-zero exit code, the installation is aborted and a **Full Transactional Rollback** is performed.
*   The system is left 100% clean.

### 2. Automatic Extraction
The engine automatically identifies and extracts files defined as hooks, even if they are missing from the `files` array in the manifest.

### 3. Subdirectory Support
Hooks are not restricted to the package root. You can organize your scripts into subdirectories (e.g., `scripts/post-install.ps1`). 
*   **Path Normalization:** The engine is "Slash-Agnostic"—it automatically handles both forward-slashes (`/`) and backslashes (`\`).
*   **Auto-Provisioning:** If a hook resides in a subdirectory, the engine will automatically create the required folder structure during the extraction phase.

### 4. Execution Environment
Hooks are executed in an elevated PowerShell session (if the manager was elevated). 
*   **WorkingDirectory:** The hook's working directory is the package installation folder (`C:\roms\<name>`).
*   **Logs:** All hook output is captured and redirected to the per-package log file in `C:\roms\logs`.

---

## 🧩 Example: Post-Install Setup

**File:** `post-install.ps1`
```powershell
Write-Host "[SETUP] Initializing user configuration..." -ForegroundColor Cyan

$configPath = Join-Path $PSScriptRoot "config.json"
if (!(Test-Path $configPath)) {
    # Generate default config
    @{ version = "1.0"; theme = "dark" } | ConvertTo-Json | Out-File $configPath
}

exit 0 # Success
```
