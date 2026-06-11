# Manifest Specification

The `roms_package.json` file is the definitive manifest for any ROMs-util package. It defines how the package is identified, what it provides, and how it interacts with the ecosystem.

---

## 📂 File Location
The manifest MUST be located at the root of the package directory and included in the `.rms` (ZIP) archive.

---

## 🛠️ Schema Definition

### Root Fields

| Field | Type | Required | Description |
| :--- | :--- | :--- | :--- |
| `name` | string | **Yes** | Unique identifier (e.g., `autofirewall`). Used as the installation folder name. |
| `version` | string | **Yes** | Semantic version (e.g., `1.0.0`). |
| `commandName` | string | **Yes** | The primary command to be registered in the System PATH. |
| `executable` | string | **Yes** | Path to the entry point relative to the package root. |
| `description` | string | No | High-level summary of the tool's purpose. |
| `dependencies` | array | No | List of package names required for this tool to function. |
| `hooks` | object | No | Custom lifecycle scripts (Trinity v1.1.0+). See [Lifecycle Hooks](./lifecycle-hooks.md). |
| `environment_variables` | object | No | Key-value pairs of persistent system settings. |
| `files` | array | **Yes** | List of all files to be extracted and managed. |
| `priority` | integer | No | Default priority for the Alternatives system (Default: `100`). |

---

## 📂 Installation Hierarchy
ROMs-util enforces a standardized directory structure to ensure reliability:
*   **Root:** All packages are installed into `C:\roms\<name>`.
*   **Name-Based Standard:** The `installDir` field is removed. The engine automatically derives the folder name from the package `name` field.

---

ROMs-util enforces a standardized directory structure to ensure reliability:
*   **Root:** All packages are installed into `C:\roms\<name>`.
*   **Relocation:** The `installDir` field is deprecated. The engine automatically handles path anchoring to ensure packages are 100% relocatable.

---

## 🛡️ Dependency Resolution

ROMs-util uses an **Atomic AVC** model for dependency resolution.
*   **Recursive Mapping:** The manager crawls the `dependencies` array and builds a full tree.
*   **SemVer Compliance:** Supports robust version pinning using Caret (`^`), Tilde (`~`), logical ranges (e.g., `>=1.2.0`), and explicit equality (`=`). The manager automatically resolves the highest satisfying version from all registered sources.

---

## 🔄 The Alternatives System

The `commandName` and `priority` fields determine how the tool is registered in `C:\roms\bin`.

*   If two packages provide the same `commandName`, the one with the higher **priority** becomes the active provider.
*   Users can manually override this using `roms select <commandName>`.

---

## 🧩 Example Manifest

```json
{
    "name": "autofirewall",
    "version": "1.0.0",
    "commandName": "autofirewall",
    "executable": "bin/firewall.ps1",
    "description": "Automates Windows Firewall policy creation.",
    "author": "ROMs-util Team",
    "architecture": "all",
    "priority": 200,
    "dependencies": {
        "packages": ["dotnet-sdk-8"]
    },
    "environment_variables": {
        "FIREWALL_MODE": "STRICT"
    },
    "hooks": {
        "postInstall": "scripts/setup.ps1",
        "preUninstall": "scripts/cleanup.ps1"
    },
    "files": [
        "roms_package.json",
        "bin/firewall.ps1",
        "scripts/setup.ps1",
        "scripts/cleanup.ps1",
        "LICENSE"
    ]
}
```

---

## 🛡️ Validation
The `package_builder` tool automatically validates your manifest against this schema before allowing a build. This ensures that every `.rms` file in the ecosystem is predictable and safe.

---

## ⚠️ Common Gotchas (FAQ)

### "Should I include a 'v' in the version string?"
**No.** This is the most common mistake. 
*   ❌ **Incorrect:** `"version": "v1.0.0"`
*   ✅ **Correct:** `"version": "1.0.0"`

The leading `v` is a human-friendly label used for Git tags and Release titles. However, the `roms` resolver is a mathematical engine; it needs pure numbers to calculate version ranges and dependencies. Including a `v` will make your package unparseable and break dependency resolution.
