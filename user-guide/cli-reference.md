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
Synchronizes the local cache with all remote registries defined in `sources.json`.
*   **Example:** `roms update`

### `search <query>`
Queries the local registry cache for packages matching the name or description.
*   **Example:** `roms search firewall`

### `select <command> [auto|pkg]`
Interactive or direct management of the Alternatives system. See [Managing Alternatives](alternatives.md).

---

## 🚩 Global Flags

These flags can be appended to almost any command:

| Flag | Alias | Description |
| :--- | :--- | :--- |
| `--yes` | `-y` | Auto-confirm all prompts (Industrial Strength automation). |
| `--verbose` | `-v` | Enable detailed debug logging to the console. |

---

## 🛡️ SemVer Support

ROMs-util supports full SemVer 2.0 range resolution in the `install` command:

*   **Caret (`^`):** `pkg:^1.2.3` (Allows updates that do not change the left-most non-zero digit).
*   **Tilde (`~`):** `pkg:~1.2.3` (Allows patch-level updates).
*   **Ranges:** `pkg:>=1.0.0 <2.0.0` (Explicit logical ranges).

**Shell Safety:** The CLI is hardened against CMD and PowerShell character mangling. Special characters like `^` and `>` are automatically protected during execution.
