# Managing Alternatives

The Alternatives system is a powerful orchestration layer that allows multiple versions or providers of the same tool to coexist on your machine without command-name collisions.

---

## 🏗️ How it Works

When a package is installed, it registers its primary command (e.g., `test-cmd`) in the **Alternatives Database** (`alternatives.json`).
*   Instead of copying the tool directly to the PATH, ROMs-util creates a **Shim** (a small `.bat` launcher) in `C:\roms\bin`.
*   The shim acts as a pointer to the "Active Provider."

---

## 🚦 Operation Modes

### 1. Automatic Mode (Default)
In Auto Mode, the system uses the **Priority** defined in the package manifest to decide which provider is active.
*   If you install a newer version or a better tool with a higher priority, the system automatically "pivots" the shim to point to the new tool.
*   **Rule:** The system only pivots if the new candidate has a **strictly higher** priority than the current one.

### 2. Manual Mode (Locked)
Users can "lock" a command to a specific package, regardless of priority.
*   Once a command is in Manual Mode, new installations will **not** pivot the shim.
*   It stays locked until the user explicitly reverts it to Auto Mode or uninstalls the locked provider.

---

## 🛠️ The `select` Command

Use `roms select` to manage these mappings.

### Interactive Selection
Run `roms select` without arguments to see a list of all managed commands.
Run `roms select <command>` to see all available providers for that command and pick one by number.

### Direct Locking
`roms select <command> <package-id>`
*   **Example:** `roms select node node-18.0.0`

### Reverting to Auto
`roms select <command> auto`
*   **Example:** `roms select node auto`

---

## 🛡️ Fail-Safe: Auto-Pivot

One of the core benefits of this system is its resilience during uninstallation.
*   If you uninstall the **Active Provider**, the Manager automatically identifies the "Next Best" candidate based on priority.
*   It pivots the shim to the new provider immediately, ensuring your environment never breaks.
*   If no other providers exist, the shim is surgically removed.
