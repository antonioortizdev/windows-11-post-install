# Windows 11 Post-Install Toolkit — Performance, Low-Latency & System Hardening

> ⚠️ **IMPORTANT — Create a System Restore Point First**
> Before applying any of the tweaks in this guide, create a restore point so you can easily roll back in case something causes instability or unexpected behavior.

**How to create a restore point**

1. Press **Win + R** → type `SystemPropertiesProtection` → Enter
2. Select your system drive (normally **C:**)
3. Click **Create**
4. Enter a name such as *Before Performance Tweaks*
5. Click **Create** and wait for confirmation

Once the restore point is created, you may proceed with the tweaks below.

---

## Table of Contents

- [0. Baseline FPS Check](#0-baseline-fps-check)
- [1. Windows Settings (No Reboot Required)](#1-windows-settings-no-reboot-required)
  - [1.0 File Explorer — Enable Visibility & File Awareness](#10-file-explorer--enable-visibility--file-awareness)
  - [1.1 Disable Mouse Acceleration (“Enhance pointer precision”)](#11-disable-mouse-acceleration-enhance-pointer-precision)
  - [1.2 Disable General Privacy “Let…” Options](#12-disable-general-privacy-let-options)
  - [1.3 Enable Game Mode & High-Performance GPU](#13-enable-game-mode--high-performance-gpu)
  - [1.4 Power Plan: Ultimate / Maximum Performance](#14-power-plan-ultimate--maximum-performance)
  - [1.5 Disable Offline Maps Automatic Updates](#15-disable-offline-maps-automatic-updates)
  - [1.6 Adjust Windows Visual Effects for Performance](#16-adjust-windows-visual-effects-for-performance)
- [2. Windows Tweaks Requiring a Restart (services.msc + Registry)](#2-windows-tweaks-requiring-a-restart-servicesmsc--registry)
  - [2.1 Enable Hardware-Accelerated GPU Scheduling (HAGS)](#21-enable-hardware-accelerated-gpu-scheduling-hags)
  - [2.2 Disable Fast Startup](#22-disable-fast-startup)
  - [2.3 Disable Connected User Experiences and Telemetry (DiagTrack)](#23-disable-connected-user-experiences-and-telemetry-diagtrack)
  - [2.4 NetworkThrottlingIndex](#24-networkthrottlingindex)
  - [2.5 MenuShowDelay (UI menu delay)](#25-menushowdelay-ui-menu-delay)
- [3. Debloat & Scripts (Advanced)](#3-debloat--scripts-advanced)
  - [3.1 Remove Windows AI Copilot Provider](#31-remove-windows-ai-copilot-provider)
  - [3.2 Remove Xbox Game Bar / Gaming Overlay (optional)](#32-remove-xbox-game-bar--gaming-overlay-optional)
- [4. BIOS & Hardware Tweaks](#4-bios--hardware-tweaks)
  - [4.1 Enable XMP / DOCP / EXPO for RAM](#41-enable-xmp--docp--expo-for-ram)
  - [4.2 Enable Resizable BAR](#42-enable-resizable-bar)
  - [4.3 Fan Curves (Q-Fan / Smart Fan)](#43-fan-curves-q-fan-smart-fan)
- [5. GPU Drivers & Control Panel](#5-gpu-drivers--control-panel)
  - [5.1 Clean Driver Install with DDU](#51-clean-driver-install-with-ddu)
  - [5.2 NVIDIA Control Panel for Performance](#52-nvidia-control-panel-for-performance)
  - [5.3 AMD Adrenalin for Performance](#53-amd-adrenalin-for-performance)
- [6. Software Blacklist & App Settings](#6-software-blacklist--app-settings)
  - [6.1 Third-Party Antivirus](#61-third-party-antivirus)
  - [6.2 “Optimizer” Tools (CCleaner, Driver Booster, etc.)](#62-optimizer-tools-ccleaner-driver-booster-etc)
  - [6.3 Discord Settings](#63-discord-settings)

---

> DISCLAIMER
> These tweaks are intended for advanced users.
> Editing system security, registry, startup configuration, BIOS or drivers may cause instability or reduce protection.
> Always back up settings or create a restore point before applying changes.

---

## 0. Baseline FPS Check

Before changing anything, measure your current in-game performance so you can tell whether these tweaks actually helped.

1. Launch your main game (e.g. Warzone, Valorant, Fortnite).
2. Join a match or practice mode.
3. Stand still in a reasonably demanding area and note your **average FPS**.
4. Write it down (e.g. *Before tweaks: 90 FPS*).

After finishing your tweaks, repeat this test in the same spot and compare.

---

## 1. Windows Settings (No Reboot Required)

### 1.0 File Explorer — Enable Visibility & File Awareness

**Where:**
File Explorer → Three dots (…) → **Options** → Folder Options → **View** tab

**What it does:**
Shows hidden files and file extensions so you always know exactly what you are editing — especially important when working with scripts, registry exports, configuration files, and system folders.

**Steps**

1. Open **File Explorer**.
2. Click the **… (three dots)** button → select **Options**.
3. Go to the **View** tab.
4. Under **Hidden files and folders** → select:
   - **Show hidden files, folders, and drives**
5. Disable (untick):
   - **Hide extensions for known file types**
6. Click **Apply** → **OK**.

**Restart required:** No (applies immediately).

**Why this matters**

- Prevents confusion between similarly-named files.
- Makes it easier to identify scripts vs documents.
- Helps avoid editing the wrong file or location.
- Essential for safe system tweaking.

---

### 1.1 Disable Mouse Acceleration (“Enhance pointer precision”)

**Where:**
Settings → Bluetooth & devices → Mouse → Additional mouse settings

**What it does:**
Disables Windows mouse acceleration so your mouse movement becomes 1:1. This is critical for consistent aim and muscle memory in shooters.

**Steps**

1. Open **Settings → Bluetooth & devices → Mouse**.
2. Click **Additional mouse settings**.
3. In the **Pointer Options** tab, uncheck **Enhance pointer precision**.
4. Click **Apply** → **OK**.

**Restart required:** No (takes effect immediately).

---

### 1.2 Disable General Privacy “Let…” Options

**Where:**
Settings → Privacy & security → General

**What it does:**
Turns off various personalization and tracking options that use your activity data, app launches and advertising ID.

**Recommended configuration**

Turn **Off** all toggles that start with **“Let …”**, for example:

- Let apps show me personalized ads by using my advertising ID
- Let websites show me locally relevant content
- Let Windows improve Start and search results by tracking app launches
- Show me suggested content in the Settings app

**Steps**

1. Open **Settings → Privacy & security → General**.
2. Turn **Off** each toggle under this section.

**Restart required:** No.

---

### 1.3 Enable Game Mode & High-Performance GPU

**Where:**
Settings → Gaming → Game Mode
Settings → System → Display → Graphics

**What it does:**
Game Mode prioritizes game processes over background tasks and can reduce interference from Windows Update and other services. Setting your main game to *High Performance* ensures it always uses the dedicated GPU.

**Steps**

1. Open **Settings → Gaming → Game Mode** and turn **Game Mode** **On**.
2. Go to **Settings → System → Display → Graphics**.
3. Add your main game executable if it is not listed.
4. Click the game → **Options** → choose **High performance** → **Save**.

**Restart required:** No.

---

### 1.4 Power Plan: Ultimate / Maximum Performance

**Where:**
Power plan creation via PowerShell, then classic Power Options UI.

**What it does:**
Enables the hidden **Ultimate / Maximum Performance** power plan so the CPU does not aggressively downclock or park cores, improving minimum FPS and responsiveness (especially on desktops).

**Steps**

1. Open **PowerShell** as Administrator (Win key → type `PowerShell` → right-click → **Run as administrator**).
2. Run:

   ```powershell
   powercfg -duplicatescheme e9a42b02-d5df-448d-aa00-03f14749eb61
   ```

3. Press **Win + R**, type `powercfg.cpl`, press Enter.
4. Under **Show additional plans**, select **Ultimate performance** (or **Maximum performance**).

**Restart required:** No (but recommended after major power plan changes).

**How to revert**

Switch back to **Balanced** or your previous plan in the same window.

---

### 1.5 Disable Offline Maps Automatic Updates

**Where:**
Settings → Apps → Offline maps → Map updates

**What it does:**
Prevents automatic offline map downloads in the background, reducing network and disk activity.

**Steps**

1. Open **Settings**.
2. Go to **Apps → Offline maps**.
3. Under **Map updates**, turn off **Update automatically when plugged in and on Wi-Fi**.

**Restart required:** No.

---

### 1.6 Adjust Windows Visual Effects for Performance

**Where:**
Control Panel → System → Advanced system settings → Performance

**What it does:**
Disables most Windows UI animations and visual effects so that resources are prioritized for applications and games rather than desktop rendering.

**Steps**

1. Press **Win + R**, type `SystemPropertiesPerformance`, press Enter.
2. In the **Visual Effects** tab, select **Adjust for best performance**.
3. (Optional, recommended) Re-enable:
   - **Smooth edges of screen fonts**
   if text looks too harsh or pixelated.
4. Click **Apply** → **OK**.

**Restart required:** No (takes effect immediately).

**Notes**

- This tweak mainly affects UI responsiveness and background composition.
- It does not increase raw FPS, but can reduce desktop overhead on very low-end systems.

---

## 2. Windows Tweaks Requiring a Restart (services.msc + Registry)

### 2.1 Enable Hardware-Accelerated GPU Scheduling (HAGS)

**Where:**
Settings → System → Display → Graphics → Default graphics settings

**What it does:**
Allows the GPU to handle its own scheduling queue, which can reduce latency and improve frame pacing on supported systems.

**Steps**

1. Open **Settings → System → Display**.
2. Click **Graphics**.
3. Click **Default graphics settings** (or **Advanced graphics settings**).
4. Turn **Hardware-accelerated GPU scheduling** **On**.

You can wait to reboot until after finishing 2.2–2.5.

**Restart required:** Yes (effective after reboot).

**Note:**
If you observe stuttering or instability, turn this option **Off** and reboot.

---

### 2.2 Disable Fast Startup

**Where:**
Control Panel → Hardware and Sound → Power Options → System Settings
`Control Panel\Hardware and Sound\Power Options\System Settings`

**What it does:**
Fast Startup performs a hybrid shutdown (partial hibernation). Disabling it forces a full shutdown so the kernel and drivers are fully reinitialized on boot, which can improve stability and avoid carrying over “stale” state.

**Steps**

1. Open **Control Panel**.
2. Go to **Hardware and Sound → Power Options**.
3. Click **Choose what the power buttons do** (System Settings).
4. Click **Change settings that are currently unavailable**.
5. Under **Shutdown settings**, uncheck **Turn on fast startup (recommended)**.
6. Click **Save changes**.

You can wait to reboot until after finishing 2.3–2.5.

**Restart required:** Yes (full reboot recommended).

**How to revert**

Re-check **Turn on fast startup (recommended)** and save.

---

### 2.3 Disable Connected User Experiences and Telemetry (DiagTrack)

**Where:**
`services.msc` → **Connected User Experiences and Telemetry** (service name `DiagTrack`)

**What it does:**
Stops a service that collects diagnostic and telemetry data for Microsoft. Mainly a privacy / background activity tweak.

**Steps**

1. Press **Win + R**, type `services.msc`, press Enter.
2. Locate **Connected User Experiences and Telemetry**.
3. Double-click it.
4. Set **Startup type** to **Disabled**.
5. Click **Stop** if the service is running.
6. Click **Apply** → **OK**.

Do **not** reboot yet — finish the registry tweaks below and then reboot once.

**Restart required:** Yes (after doing 2.4 & 2.5).

**How to revert**

1. Open `services.msc` again.
2. Set **Startup type** to **Manual** or **Automatic**.
3. Restart Windows.

---

> ⚠️ **Registry note**
> You already have a system restore point, but still:
> before editing keys, you can **export** each key (`File → Export` in Regedit) so you can restore it quickly if needed.

### 2.4 NetworkThrottlingIndex

**Where (Registry):**
`HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Multimedia\SystemProfile`

**What it does:**
Controls Windows’ multimedia network throttling. By default, it limits how aggressively non-multimedia network traffic is processed to keep CPU time available for audio/video tasks.

**Recommended values**

| Use Case                  | Value           |
|---------------------------|-----------------|
| Default Windows behavior  | `10` (decimal)  |
| Gaming / latency focus    | `FFFFFFFF` (hex)|

> **Note:** By default you will usually see the value set to `10` (shown as `A` in hexadecimal). This is standard Windows behavior.

**Steps**

1. Press **Win + R**, type `regedit`, press Enter.
2. Navigate to the key above.
3. Right-click **SystemProfile → Export** to create a backup.
4. Create or edit the **DWORD (32-bit)** value `NetworkThrottlingIndex`.
5. Set **Base** to **Hexadecimal** and **Value** to `FFFFFFFF`.

Do **not** reboot yet — finish 2.5 and then reboot once.

**Restart required:** Yes (recommended after 2.5).

**Context – what many guides & videos claim**

> “Network Throttling Index limits how fast network packets are processed to prioritize multimedia tasks (like video playback). Setting the value to `FFFFFFFF` disables this limitation to avoid latency issues in competitive games.”

This guide explains both the tweak and how to revert it safely.

**How to revert**

- Set the value back to `10` (Decimal), or
- Delete the value to restore default behavior, then restart.

---

### 2.5 MenuShowDelay (UI menu delay)

**Where (Registry):**
`HKEY_CURRENT_USER\Control Panel\Desktop`

**What it does:**
Reduces or removes the delay before Windows opens menus, making the desktop feel snappier (pure UX).

**Default:** `400` (milliseconds)
**Suggested:** `0` (no delay)

**Steps**

1. In **Regedit**, navigate to `HKEY_CURRENT_USER\Control Panel\Desktop`.
2. Double-click `MenuShowDelay`.
3. Change the value from `400` to `0`.
4. Close Regedit.

Now perform **one reboot** to apply 2.1–2.5 in one go.

**Restart required:** Yes (or sign-out).

**How to revert**

Change `MenuShowDelay` back to `400`.

---

## 3. Debloat & Scripts (Advanced)

### 3.1 Remove Windows AI Copilot Provider

**Where:**
PowerShell / Terminal (Administrator)

**What it does:**
Uninstalls the `Microsoft.Windows.Ai.Copilot.Provider` UWP package, removing Copilot-related background integration. This is a **privacy / debloat** tweak, not a performance tweak.

**Steps**

1. Right-click the **Start** button.
2. Select **Windows PowerShell (Admin)** or **Terminal (Admin)**.
3. Run:

   ```powershell
   Get-AppxPackage -AllUsers Microsoft.Windows.Ai.Copilot.Provider | Remove-AppxPackage
   ```

4. Close the window.
5. Restart Windows.

**Restart required:** Recommended.

**How to revert**

1. Run **Windows Update**; major feature updates typically restore this package, **or**
2. Re-register the package (if still present on disk):

   ```powershell
   Add-AppxPackage -Register -DisableDevelopmentMode "C:\Program Files\WindowsApps\Microsoft.Windows.Ai.Copilot.Provider*\AppxManifest.xml"
   ```

(Use the actual folder path if multiple versions exist.)

---

### 3.2 Remove Xbox Game Bar / Gaming Overlay (optional)

**Where:**
PowerShell / Terminal (Administrator)

**What it does:**
Uninstalls the `Microsoft.XboxGamingOverlay` package. Only use this if you never use Game Bar (`Win + G`) for capture or overlays.

**Steps**

1. Open **PowerShell (Admin)**.
2. Run:

   ```powershell
   Get-AppxPackage -AllUsers Microsoft.XboxGamingOverlay | Remove-AppxPackage
   ```

3. Restart Windows (optional but recommended).

**Restart required:** No (but recommended).

**How to revert**

1. Open **Microsoft Store**.
2. Search for **Xbox Game Bar**.
3. Click **Install**.

---

## 4. BIOS & Hardware Tweaks

> These changes are done in your motherboard’s BIOS/UEFI. If you are on a locked laptop BIOS, some options may not be available.fileciteturn0file0

### 4.1 Enable XMP / DOCP / EXPO for RAM

**What it does:**
Ensures your RAM runs at its advertised speed (e.g. 3200 MHz, 6000 MHz). Without XMP/DOCP/EXPO, it typically runs at a much lower default frequency (2133/4800 MHz).

**Steps**

1. Reboot your PC and repeatedly press **Del** or **F2** to enter BIOS.
2. On the main screen, look for **XMP** (Intel), **DOCP** (older AMD) or **EXPO** (newer AMD).
3. Set it from **Disabled** to **Profile 1** / **XMP I**.
4. Save and exit (usually **F10**).

**Notes**

- Ignore “Profile 2 / XMP II” if unstable.
- This mainly improves 1% lows and reduces stutter.

---

### 4.2 Enable Resizable BAR

**What it does:**
Allows the CPU to access the GPU memory in larger chunks, improving performance on modern GPUs (RTX 3000+ / RX 6000+).

**Steps**

1. Enter BIOS.
2. Find settings under **Advanced** or **PCIe Settings**.
3. Enable **Above 4G Decoding**.
4. Enable **Re-Size BAR Support**.
5. Save and exit (F10).

---

### 4.3 Fan Curves (Q-Fan / Smart Fan)

**What it does:**
Configures fan curves so the system stays quiet at idle but ramps up under load, reducing thermal throttling.

**Typical target curve**

- Up to 50°C → ~30% fan speed (quiet).
- Around 70°C → ~70% fan speed (gaming load).
- 85°C and above → 100% fan speed (emergency).

**Steps**

1. Enter BIOS.
2. Look for **Q-Fan Control**, **Smart Fan** or similar.
3. Set all case fans to **PWM** mode (not DC, if supported).
4. Adjust the curve roughly as above and save.

---

## 5. GPU Drivers & Control Panel

### 5.1 Clean Driver Install with DDU

**What it does:**
Removes old GPU drivers and installs the latest ones cleanly, avoiding conflicts and leftover files.

**Requirements**

- Latest driver installer downloaded from **NVIDIA** or **AMD**.
- **DDU (Display Driver Uninstaller)**.

**Protocol**

1. Download the latest GPU driver but **do not install it yet**.
2. Download and extract latest version of DDU [here](https://www.wagnardsoft.com/forums/viewtopic.php?t=5471).

Follow this guide step by step. [How to use DDU tutorial.](https://www.wagnardsoft.com/content/How-use-Display-Driver-Uninstaller-DDU-Guide-Tutorial)

---

### 5.2 NVIDIA Control Panel for Performance

**Where:**
Right-click desktop → **NVIDIA Control Panel** → **Manage 3D settings**

**Suggested global settings**

- **Low Latency Mode:** `On` or `Ultra` (if your GPU is often at 99%).
- **Power management mode:** `Prefer maximum performance`.
- **Texture filtering – Quality:** `High performance`.
- **Vertical sync:** `Off` (use in-game V-Sync or G-Sync/Freesync instead, if desired).fileciteturn0file0

You can also set these per-game in the **Program Settings** tab.

---

### 5.3 AMD Adrenalin for Performance

**Where:**
AMD **Adrenalin** → **Gaming → Graphics**

**Suggested settings**

- Use the **eSports** profile as a baseline.
- **Radeon Anti-Lag:** On.
- **Radeon Chill / Boost:** Off.
- **Tessellation Mode:** Override application settings → Off (for maximum FPS in many games).fileciteturn0file0

---

## 6. Software Blacklist & App Settings

### 6.1 Third-Party Antivirus

**Problem:**
Suites like Norton, McAfee, Avast, etc. hook deeply into the system, scanning everything in real time and consuming CPU.

**Recommendation**

- Uninstall heavy third-party antivirus suites.
- Use **Microsoft Defender** (built into Windows) plus sane browsing habits instead.fileciteturn0file0

---

### 6.2 “Optimizer” Tools (CCleaner, Driver Booster, etc.)

**Problem:**
Many “one-click optimizers” run background services, install questionable drivers, or collect data.

Examples:

- CCleaner (modern versions)
- Driver Booster / Easy Driver
- Other “PC Accelerator” utilities

**Recommendation**

- Uninstall them.
- Use native tools (Disk Cleanup, Storage Sense) and manual driver installs from vendor sites.

---

### 6.3 Discord Settings

**What it does:**
Adjusts Discord so it does not steal FPS or add latency while gaming.

**Steps**

1. Open **Discord → User Settings** (gear icon).
2. Go to **Game Overlay**.
   - Disable **Enable in-game overlay**.
3. Go to **Advanced**.
   - **Hardware Acceleration:**
     - If your CPU is weak and GPU is strong → **On**.
     - If your GPU is maxed out at 99% or Discord feels sluggish → try **Off** and test.

These changes reduce unnecessary overlay rendering and potential input lag.

---

If you want, we can now turn this into two profiles inside the same file:

- **Safe baseline** (only the non-invasive tweaks)
- **Aggressive competitive mode** (adds debloat, VBS off, AppX removals, etc.).
