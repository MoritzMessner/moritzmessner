---
layout: default
title: "How to use Puro for Flutter version management"
date: 2025-10-04
description: "Puro ist ein Tool zum Installieren und Verwalten verschiedener Flutter-Versionen – ideal für Projekte mit unterschiedlichen Anforderungen."
---

Puro is a powerful tool for **installing and managing different versions of Flutter** on your computer. It's especially useful if you work on multiple projects that require different Flutter versions, as it speeds up downloads and saves disk space.

**Why Puro?**
- 20% faster initial setup compared to traditional methods
- 50-95% faster subsequent installations through global caching
- Automatic IDE configuration with a single command
- Per-project or global version management

Here is a simple tutorial on how to get started.

---

## 1. Installation

Puro can be installed on Windows, Linux, or Mac.

**Prerequisite:** Make sure you have **Git** installed on your system, as Flutter requires it.

**Windows:**

Run in PowerShell (as current user, not Administrator):

```powershell
Invoke-WebRequest -Uri "https://puro.dev/builds/1.5.0/windows-x64/puro.exe" -OutFile "$env:temp\puro.exe"; &"$env:temp\puro.exe" install-puro --promote
```

**Linux/Mac:**

Run in terminal:

```bash
curl -o- https://puro.dev/install.sh | PURO_VERSION="1.5.0" bash
```

If the installation fails, check out the official documentation at [puro.dev](https://puro.dev/).

---

## 2. Quick Start: Basic Usage

Once installed, you can use the `puro` command to manage Flutter versions.

### A. Install Flutter

To install the **latest stable version** of Flutter:

```bash
puro flutter doctor
```

This checks your system and automatically installs the latest stable Flutter if missing.

### B. Manage Environments

Puro uses **environments** to keep different versions separate. The default environment is `stable`.

| Command | Purpose |
| ------- | ------- |
| `puro ls` | Lists all installed Puro environments. |
| `puro releases` | Lists all available Flutter versions and channels. |
| `puro create myapp-dev 3.10.6` | Creates environment `myapp-dev` with Flutter 3.10.6. |
| `puro use myapp-dev` | Switches current project to `myapp-dev`. |
| `puro use -g beta` | Switches global default to beta channel. |
| `puro rm old-env` | Deletes an environment. |

### C. Running Flutter Commands

After setting an environment, run Flutter commands as usual:

| Command | Purpose |
| ------- | ------- |
| `puro flutter run` | Runs app with the set Flutter version. |
| `puro dart upgrade` | Runs dart upgrade in the managed environment. |
| `puro -e myapp-dev flutter run` | Uses a specific environment without changing settings. |

---

## 3. Real-World Example: HL-Hypnosen App

Here's how I use Puro for my [HL-Hypnosen App](https://apps.apple.com/de/app/hl-hypnosen/id1641096135) – a Flutter app with 3,800+ users on iOS and Android.

**Setup a dedicated environment:**

```bash
# Create an environment with a specific Flutter version
puro create hl-hypnosen 3.24.0

# Navigate to project and set the environment
cd ~/projects/hl-hypnosen
puro use hl-hypnosen
```

**Daily workflow:**

```bash
# Run the app
puro flutter run

# Build for release
puro flutter build appbundle --release
puro flutter build ipa --release
```

**Why this matters:**

The HL-Hypnosen app uses RevenueCat, Firebase, and several other packages that sometimes require specific Flutter versions. With Puro, I can:

- Keep a stable Flutter version for production builds
- Test new Flutter releases in a separate environment without risk
- Switch between projects instantly without re-downloading SDKs

```bash
# Quick version check
puro ls

# Output:
# * hl-hypnosen (3.24.0) [current]
#   stable (3.24.5)
#   beta (3.25.0-beta)
```

This setup has saved me hours of debugging version conflicts across my projects.

---

For a full list of commands, check out the [Puro Manual](https://puro.dev/reference/manual/).

<small>Veröffentlicht am {{ page.date | date: "%d.%m.%Y" }}</small>
