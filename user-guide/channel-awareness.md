# Channel Awareness

Channel awareness is a core feature of the ROMs-util ecosystem that allows users and developers to partition packages into logical streams, such as `mainnet` (stable) and `testnet` (experimental). This ensures system stability by isolating untested software while still allowing for easy side-loading and testing.

---

## 🏗️ The Partitioned Model

Unlike traditional managers that use a single flat registry, ROMs-util uses **Partitioned Indices**. Every source repository can host multiple channels, each with its own `index.json` file.

*   **Mainnet:** The default, production-ready channel.
*   **Testnet:** The experimental channel for beta testing and early access.

---

## 🛠️ Managing Channels (`roms source`)

The `source` command suite is the primary interface for managing your registry configuration.

### 1. Listing Channels
`roms source list`
Displays all registered sources and the status of their channels. Channels marked with an asterisk (`*`) are active in the current session.

### 2. Global Toggling (Persistent)
`roms source on <channel>` / `roms source off <channel>`
Globally enables or disables a channel across all sources. 
*   **Note:** This modifies `sources.json` and requires **Administrator privileges**.
*   Disabled channels are ignored during `roms update` and `roms search`.

### 3. Session Picking (Transient)
`roms source pick <channel>`
Temporarily activates a channel for the **current window only**.
*   **Zero Impact:** Does not require Administrator privileges and does not change your global settings.
*   **Window Isolated:** Uses **Ancestor Shell Detection** to ensure the setting only applies to the terminal window where you ran the command.
*   **Temporary Activation:** Even if a channel is globally "OFF", `pick` will cause `roms update` and `roms search` to treat it as "ON" for that specific window.

---

## 🛡️ Channel Isolation Mandate

To prevent "Experimental Pollution," ROMs-util enforces a strict isolation mandate:
1.  **Invisible Cache:** Even if a channel's index is present in the local cache folder, the manager will **ignore** it unless that channel is currently active (ON or PICKED).
2.  **Surgical Sync:** `roms update` only fetches data for authorized channels, saving bandwidth and maintaining a clean system state.

---

## 🧩 Practical Workflow: Testing a Beta

If you want to test a package that is only available on `testnet` without changing your system defaults:

```powershell
# 1. Pick the testnet for this window
roms source pick testnet

# 2. Sync the testnet index (only happens for this session)
roms update

# 3. Install the package
roms install my-beta-tool

# 4. (Optional) Open a new window and run 'roms list'
# The testnet channel will be invisible there, maintaining isolation.
```
