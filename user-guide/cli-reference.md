# CLI Reference

The `roms` command-line interface is the primary way users interact with the ROMs-util ecosystem. It is designed to be professional, direct, and shell-safe.

---

## 🛠️ Usage Patterns

All commands follow the standard pattern:
`roms <command> [arguments] [flags]`

---

## 📦 Core Commands

### `install <name|path>`
Installs a package and its dependencies.
*   **By Name:** `roms install autofirewall` (Fetches latest from registry).
*   **By Version:** `roms install helper:^1.1.0` (Supports Caret, Tilde, and Ranges).
*   **By Path:** `roms install .\my-package.rms` (Installs a local file).

### `uninstall <name>`
Surgically removes a package, its metadata, and its registered shims.
*   **Example:** `roms uninstall autofirewall`

### `list`
Lists all installed packages, their versions, and their primary commands.
*   **Example:** `roms list`

### `update`
Synchronizes the local cache with all active and session-picked registries. Non-elevated.
*   **Example:** `roms update`

### `source <list|on|off|pick>`
Manages registry channels and session-specific activation.
*   **`list`**: Show statuses of all registered channels.
*   **`on/off`**: Globally toggle channel visibility (Requires UAC).
*   **`pick`**: Temporarily activate a channel for the current window.

### `search <query>`
Queries the local cache for packages in active channels.

### `select <command> [auto|pkg]`
Interactive or direct management of the Alternatives system. See [Managing Alternatives](alternatives.md).

---

## 🚩 Global Flags

These flags can be appended to almost any command:

| Flag | Alias | Description |
| :--- | :--- | :--- |
| `--yes` | `-y` | Auto-confirm all prompts (Automated execution). |
| `--verbose` | `-v` | Enable detailed debug logging to the console. |

---

## 🛡️ SemVer Support

ROMs-util supports full SemVer 2.0 resolution in the `install` command. Coordinates are **shell-safe** and do not require quotes in CMD or PowerShell.

*   **Caret (`^`):** `pkg:^1.2.3` (Allows updates that do not change the left-most non-zero digit).
*   **Tilde (`~`):** `pkg:~1.2.3` (Allows patch-level updates).
*   **Pinned (`=`):** `pkg:=1.0.0` (Explicitly locks to a specific version).
*   **Ranges:** `pkg:>=1.0.0 <2.0.0` (Explicit logical ranges).
*   **Default:** Bare version strings (e.g., `1.0.0`) are treated as **Exact Matches**.

**Shell Safety:**
The CLI implements a **Bulletproof Tunnel** (Delayed Expansion) that captures literal symbols like `^` even when unquoted in CMD. Additionally, it features **Shell Operator Isolation**, ensuring that chained commands (e.g., `roms install pkg && roms list`) are correctly handled by the shell and not misinterpreted as arguments.
