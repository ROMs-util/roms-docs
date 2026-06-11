# Environment Variables

Environment variables allow packages to persist system-level configuration that is accessible to all processes on the machine. ROMs-util manages the full lifecycle of these variables to ensure system stability and hygiene.

---

## 🏗️ The Persistence Model

Unlike session-based variables, environment variables set by ROMs-util are **persistent**.
*   **Scope:** Applied to the **Machine** scope (`HKLM:\System\CurrentControlSet\Control\Session Manager\Environment`).
*   **Persistence:** Settings survive computer reboots.
*   **Visibility:** Available to all user accounts and system services.

---

## 🛠️ Implementation Guide

To define environment variables, add the `environment_variables` object to your `roms_package.json`:

```json
{
    "name": "my-app",
    "version": "1.0.0",
    "environment_variables": {
        "MYAPP_HOME": "C:\\roms\\my-app",
        "MYAPP_LOG_LEVEL": "DEBUG"
    }
}
```

### Path Variables
If you need to point to a specific directory within your package, use the full path anchored to `C:\roms\<name>`.

---

## 🛡️ Variable Hygiene

One of the primary benefits of using ROMs-util for environment management is automatic cleanup.

### 1. Artifact Tracking
During installation, the engine (`rmspkg`) identifies every environment variable defined in the manifest and registers it as a tracked **Artifact** in the local metadata database. These are prefixed with `env:` (e.g., `env:MYAPP_HOME`).

### 2. Surgical Cleanup
When a package is uninstalled:
*   The engine reads the tracked artifacts.
*   It identifies the `env:` entries.
*   It surgically removes these specific keys from the Windows Registry.

**The result:** Your system remains 100% clean. No "orphan" environment variables are left behind to pollute your system state.

---

## 🧩 Practical Example: Java Setup

**Manifest Snippet:**
```json
{
    "name": "openjdk-21",
    "version": "21.0.2",
    "environment_variables": {
        "JAVA_HOME": "C:\\roms\\openjdk-21",
        "JDK_HOME": "C:\\roms\\openjdk-21"
    }
}
```

When this package is uninstalled, both `JAVA_HOME` and `JDK_HOME` will be removed automatically, even if they were pointing to different folders.
