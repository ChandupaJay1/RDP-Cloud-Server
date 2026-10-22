# 🛡️ Security Policy

## 📋 Overview

The **Windows RDP Runner with Android Studio & Tailscale** project is committed to ensuring the safety, privacy, and integrity of cloud-hosted virtual environments and user workflows. Because this project automates cloud runners, virtual private networks (Tailscale), and Remote Desktop (RDP) sessions, adhering to strict security practices is vital.

---

## 🔒 Supported Versions

Only the latest commit on the `main` branch is actively supported with security updates and configuration patches.

| Version / Branch | Supported | Notes |
| :--- | :---: | :--- |
| `main` | ✅ | Active development and security patches |
| Release tags | ✅ | Latest tagged release |
| Forks / Outdated commits | ❌ | Please sync with upstream `main` |

---

## 🚨 Reporting a Vulnerability

We take all security vulnerabilities seriously and appreciate responsible disclosure.

### How to Report

1. **GitHub Private Vulnerability Advisory (Recommended):**
   - Navigate to the **[Security](https://github.com/ChandupaJay1/RDP-Cloud-Server/security)** tab of this repository.
   - Click on **Advisories** > **Report a vulnerability**.
   - This creates a private channel between you and the maintainers.

2. **Email Disclosure:**
   - If private advisories are unavailable, please open a confidential inquiry or contact the repository owner directly before publishing any public details.

> [!CAUTION]
> **Please DO NOT report security vulnerabilities through public GitHub issues, discussions, or pull requests.** Public disclosure before a fix is available puts all users and workflows at risk.

### What to Include in Your Report

To help us investigate and resolve the issue quickly, please provide:
- A clear description of the vulnerability and its potential impact.
- Step-by-step instructions or a Minimal Reproducible Example (Proof of Concept).
- Specific files, workflow lines, or configuration settings involved.
- Any suggested remediations or workarounds.

### Response Timeline

- **Acknowledgment:** Within **48 hours** of report receipt.
- **Triage & Assessment:** Within **5 business days**.
- **Fix & Disclosure:** Coordinated release with reasonable remediation time before public announcement.

---

## ⚙️ Security Guidelines & Best Practices for Deployers

When deploying and using this automated runner environment, you **must** observe the following security controls:

### 1. 🔑 Tailscale VPN & Authentication Keys
- **Use Ephemeral Keys:** When generating an auth key in the [Tailscale Admin Console](https://login.tailscale.com/admin/settings/keys), enable the **Ephemeral** toggle so that runner nodes are automatically removed from your Tailnet when the workflow finishes.
- **Apply Tailscale ACLs & Tags:** Restrict the runner node’s access permissions using Tailscale ACL tags (e.g., `tag:ci-runner`). Do not grant the runner unrestricted access to your private local home or office subnets unless strictly required.
- **Store in Repository Secrets:** Always set `TAILSCALE_AUTHKEY` in **Settings > Secrets and variables > Actions**. Never hardcode or commit active auth keys into workflow YAML files.

### 2. 🔐 RDP Password & Credential Hygiene
- **Override Default Passwords:** Never run production sessions with the fallback default password. Set a strong, unique `RDP_PASSWORD` in your GitHub repository secrets.
- **Network Level Authentication (NLA):** The workflow disables NLA to ensure compatibility with various RDP clients across platforms. Because of this, authentication relies solely on user credentials and Tailscale's end-to-end WireGuard® encryption.
- **Do Not Open Public Ports:** Keep RDP access restricted strictly to Tailscale's encrypted mesh tunnel. Never use third-party tunneling utilities (e.g., ngrok) to expose port 3389 publicly without access controls.

### 3. 📜 GitHub Actions Acceptable Use Policy Compliance
- **No Cryptocurrency Mining:** Using GitHub Actions runners for cryptocurrency mining is strictly prohibited and will result in immediate account termination.
- **No Malicious Activity:** Do not use this cloud server for network scanning, DDoS attacks, unauthorized scraping, hosting malicious payloads, or torrenting.
- **Respect Runtime Limits:** GitHub-hosted runners have session execution limits (up to 6 hours max). Ensure workflows terminate gracefully and do not circumvent platform quotas.
- Review GitHub's official guidelines: [GitHub Acceptable Use Policies](https://docs.github.com/en/site-policy/acceptable-use-policies/github-acceptable-use-policies).

### 4. 🗂️ Sensitive Data & Secret Masking
- **Logs:** Ensure that sensitive tokens, personal developer credentials, or signing keystores are not printed in workflow step outputs.
- **Build Artifacts:** When building Android applications (`.apk` / `.aab`), do not leave private signing keys (`.keystore`, `.jks`) or environment secrets on public workflow artifacts.
- **Session Cleanup:** Remember that runner storage is ephemeral; any personal logins or files left on the machine are destroyed once the job completes, but session logs remain visible to repository collaborators.

---

## 📄 License & Responsible Use

This project is intended for legitimate software development, Android SDK testing, and continuous integration environments. Maintainers assume no liability for misuse of this tool in violation of GitHub's Terms of Service or local laws.
