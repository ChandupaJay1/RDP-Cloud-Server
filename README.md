# 🚀 Windows RDP Runner with Android Studio & Tailscale

A high-performance GitHub Actions workflow that provisions a cloud-hosted Windows runner equipped with **Tailscale VPN**, **Remote Desktop (RDP)**, and a fully pre-configured **Android Studio & Android SDK** development environment.

---

## 📋 Features

- 🖥️ **Windows Server Environment:** Powered by GitHub-hosted `windows-2022` runners.
- 🔒 **Secure Remote Access (Tailscale):** Direct, firewall-penetrating mesh VPN connection without opening public router ports.
- 📱 **Pre-Configured Android Studio:**
  - Installed and pre-configured to skip the first-run wizard (`disable.android.first.run=true`).
  - Bundled JetBrains Java Runtime (JBR) linked to `JAVA_HOME`.
- 🛠️ **Full Android SDK & Platform Tools:**
  - Standard system-wide location: `C:\Android\Sdk`
  - Pre-installed packages: `platform-tools` (`adb`, `fastboot`), `platforms;android-35`, and `build-tools;35.0.0`.
  - All standard Google SDK licenses pre-accepted.
  - All tools (`adb`, `fastboot`, `sdkmanager`) available globally in the system `PATH`.
- ⚡ **Zero-Config RDP Session:** User profile automation ensures directory junctions and environment variables are active immediately upon login.
- ⏱️ **Extended Uptime:** Keeps the session alive up to ~5 hours 45 minutes per run.

---

## ⚙️ Configuration & Repository Secrets

Before launching the workflow, configure the following secrets in your GitHub repository (**Settings > Secrets and variables > Actions**):

| Secret Name | Required | Description | Default Fallback |
| :--- | :---: | :--- | :--- |
| `TAILSCALE_AUTHKEY` | **Recommended** | Tailscale authentication key (Reusable/Ephemeral key from [Tailscale Admin Console](https://login.tailscale.com/admin/settings/keys)). | Pre-set fallback key |
| `RDP_PASSWORD` | Optional | Custom password for the `RDP` Windows user account. | `MR.English2008` |

---

## 🚀 How to Run

1. Place [`workflow.yaml`](workflow.yaml) into your repository under `.github/workflows/workflow.yaml`.
2. Go to the **Actions** tab in your GitHub repository.
3. Select the **`Windows 11 + Tailscale + RDP + Android Studio`** workflow.
4. Click **Run workflow** > **Branch: main** > **Run workflow**.
5. Wait for steps 1 through 8 to complete (~5–8 minutes).

---

## 🖥️ How to Connect via RDP

Once the runner reports **`RDP SERVER IS READY`** in the workflow logs:

1. **Connect to your Tailscale network:** Ensure Tailscale is installed and connected on your local PC or mobile device using the same Tailscale account.
2. **Launch Remote Desktop:**
   - Press <kbd>Win</kbd> + <kbd>R</kbd>, type `mstsc`, and press <kbd>Enter</kbd>.
3. **Enter Credentials:**
   - **Computer / Hostname:** `rdp-runner.tail406778.ts.net` *(or the Tailscale IP shown in the workflow log)*
   - **Username:** `RDP`
   - **Password:** Your `RDP_PASSWORD` secret (or default: `MR.English2008`)
4. Click **Connect**.

---

## 🛠️ Pre-Configured Android Environment Details

All environment variables and paths are set at the **System (Machine)** level, making them instantly available inside CMD, PowerShell, and Android Studio for any user:

| Variable | Value |
| :--- | :--- |
| `ANDROID_HOME` | `C:\Android\Sdk` |
| `ANDROID_SDK_ROOT` | `C:\Android\Sdk` |
| `JAVA_HOME` | `C:\Program Files\Android\Android Studio\jbr` |
| `PATH` Tools | `C:\Android\Sdk\platform-tools`<br>`C:\Android\Sdk\cmdline-tools\latest\bin`<br>`C:\Android\Sdk\emulator`<br>`C:\Program Files\Android\Android Studio\bin` |

### Included Tools & Versions:
- **Android Studio:** Latest stable
- **Platform Tools (ADB & Fastboot):** Included & verified
- **Target Platform:** `android-35` (Android 15)
- **Build Tools:** `35.0.0`
- **Command-Line Tools:** Latest (`sdkmanager`, `avdmanager`)

---

## 📂 Desktop Utilities on the RDP Session

When you log into the RDP session, your Desktop includes:

- **`Android Studio` Shortcut:** Double-click to open Android Studio directly.
- **`Test Android SDK.bat`:** A one-click verification script that displays environment variables and tests `adb version`, `sdkmanager --version`, and `java -version`.
- **Pre-installed Extras:** 7-Zip, Telegram Desktop, and system customization tools.

---

## ❓ Troubleshooting

### 1. `adb` command not recognized in a custom terminal?
The system PATH includes `C:\Android\Sdk\platform-tools`. If you open a terminal that was launched before the session finished initializing, close it and open a new Command Prompt or PowerShell window.

### 2. Android Studio asks for SDK location?
Android Studio is configured to detect `ANDROID_HOME` (`C:\Android\Sdk`). If prompted, simply point the SDK location to `C:\Android\Sdk` (or `%LOCALAPPDATA%\Android\Sdk`, which is linked via a directory junction).

### 3. Tailscale Connection Failed?
Ensure your Tailscale Auth Key is active and has sufficient device limits in your [Tailscale Console](https://login.tailscale.com/admin/machines).
