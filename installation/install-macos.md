# Installing on macOS

> **Applies to:** Syhunt Hybrid 7.2 on macOS Sequoia and earlier

**Contents**

- [Introduction](#intro)
- [Installing on macOS (Apple Silicon or Intel)](#installing)
- [Parallels Desktop: Debian ARM64](#debianarm64)
- [Parallels Desktop: Ubuntu or Kali Linux ARM64](#ubuntuarm64)
- [Parallels Desktop: enabling Rosetta](#rosetta)
- [Parallels Desktop: installing Wine 8.0](#wine8)

<a id="intro"></a>

## Introduction

Syhunt Hybrid 7.2 supports macOS Sequoia and earlier. The macOS edition is the *Carbon* edition: it is installed on top of Wine and runs on both Apple Silicon Macs (through Rosetta) and Intel Macs.

If you prefer virtualization, Syhunt Hybrid 7.1.6 also runs on ARM64 Linux (Ubuntu, Debian or Kali Linux) inside [Parallels Desktop](https://www.parallels.com/products/desktop) on macOS. This requires a Parallels **Pro** license; the Standard license is not compatible.

<a id="introend"></a>
<a id="installing"></a>

## Installing on macOS (Apple Silicon or Intel)

1. Read and accept the [Syhunt EULA](https://www.syhunt.com/en/?n=Legal.EULA) before running any of the commands below.
2. [Download Java](https://java.com/en/download/) for macOS (`jre-<version>-macosx-x64.dmg`) and install it, if you haven't already.
3. Install Wine64, if you haven't already:
   - [Apple Silicon (M1, M2, M3 or M4)](wine.md#macos)
   - [Intel](wine.md#macos_intel)
4. Install wget:
   ```bash
   brew install wget
   ```
5. Download the Syhunt setup (`.jar` file), Control-click its icon and choose **Open**. Read the EULA carefully before accepting it, then complete the installer (*Next → Next → Finish*). The Syhunt [command-line tools](../integrations/command-line.md) are then ready to use. For more options, see [Running the setup](install-linux.md#finishinstall).
6. [Enable the Syhunt Web UI](web-ui.md#firststeps).

> [!NOTE]
> If, after a macOS upgrade, scanning a Git repository fails with **invalid active developer path**, run `xcode-select --install` to repair the `git` command.

<a id="debianarm64"></a>

## Parallels Desktop: Debian ARM64

> **Applies to:** Debian 12.6 ARM64 on Apple Silicon Macs with Parallels Desktop

1. Install Java:
   ```bash
   sudo apt install default-jre
   ```
2. *(Optional)* Install Git, if you plan to scan Git repositories:
   ```bash
   sudo apt install git
   ```
3. Enable and install Rosetta (see [Enabling Rosetta](#rosetta)).
4. Install the AMD64 build of Wine64. Debian 12.6 ARM64 installs version 8.0 by default:
   ```bash
   sudo apt install wine64:amd64
   ```
5. Run `wine ipconfig` once so Wine configures itself.
6. Install Syhunt: download the `.jar` setup and follow [Running the setup](install-linux.md#finishinstall).
7. [Enable the Syhunt Web UI](web-ui.md#firststeps).

<a id="ubuntuarm64"></a>

## Parallels Desktop: Ubuntu or Kali Linux ARM64

> **Applies to:** Ubuntu 24.04 ARM64 or Kali Linux 2024.2 ARM64 on Apple Silicon Macs with Parallels Desktop

1. Read and accept the [Syhunt EULA](https://www.syhunt.com/en/?n=Legal.EULA).
2. Create a dedicated virtual machine for Syhunt.
3. Update the package lists:
   ```bash
   sudo apt-get update
   ```
4. Install Java with SVE disabled:
   ```bash
   # Add this once to the shell profile
   echo 'export JAVA_TOOL_OPTIONS="$JAVA_TOOL_OPTIONS -XX:UseSVE=0"' | sudo tee /etc/profile.d/java-nosve.sh
   # Reload the login environment (or open a new terminal)
   source /etc/profile.d/java-nosve.sh
   # Install OpenJDK 17 and confirm the version
   sudo apt install openjdk-17-jre
   java --version
   ```
5. Enable and install Rosetta (see [Enabling Rosetta](#rosetta)). Skip this step if you're using Parallels' free **Ubuntu with Rosetta** image.
6. The default Wine package is version 9, which doesn't work well with Rosetta. Enable Wine 8.0 first (see [Installing Wine 8.0](#wine8)), then install it:
   ```bash
   sudo apt install libwine:amd64=8.0~repack-4 wine64:amd64=8.0~repack-4
   ```
   If the `wine` command isn't available afterwards, run `sudo apt install wine`. Confirm the version with `wine --version`.
7. Run `wine ipconfig` once so Wine configures itself.
8. Install Syhunt: download the `.jar` setup and follow [Running the setup](install-linux.md#finishinstall).
9. [Enable the Syhunt Web UI](web-ui.md#firststeps).

<a id="rosetta"></a>

## Parallels Desktop: enabling Rosetta

> **Applies to:** Ubuntu 24.04 ARM64 or Kali Linux 2024.2 ARM64 on Apple Silicon Macs with Parallels Desktop

1. When the virtual machine has finished installing, shut it down. In the VM settings, go to **Hardware → CPU & Memory**, enable **Use Rosetta to run x86_64 binaries**, then start the VM again.
2. Install Rosetta support:
   ```bash
   wget https://kb.parallels.com/Attachments/kcs-193687/rosetta_x86_sources.sh
   chmod +x rosetta_x86_sources.sh
   sudo ./rosetta_x86_sources.sh enable    # prints "AMD64 architecture enabled"
   sudo apt update
   ```

<a id="wine8"></a>

## Parallels Desktop: installing Wine 8.0

> **Applies to:** Ubuntu 24.04 ARM64 or Kali Linux 2024.2 ARM64 on Apple Silicon Macs with Parallels Desktop

> [!WARNING]
> Use this approach in test environments only. It installs Debian 12 (*Bookworm*) packages on Ubuntu 24.04 or Kali Linux 2024.2. It currently works, but mixing packages from different distributions is not a stable long-term solution. We are working on a cleaner method and will update this guide when it's available.

1. Add the Debian 12 repository, pinned so it's used only for Wine:
   ```bash
   echo 'deb http://deb.debian.org/debian bookworm main contrib non-free non-free-firmware' |
     sudo tee /etc/apt/sources.list.d/debian-bookworm.list

   sudo tee /etc/apt/preferences.d/99-wine8 <<'EOF'
   Package: wine* libwine* fonts-wine*
   Pin: release n=bookworm
   Pin-Priority: 990

   Package: *
   Pin: release n=bookworm
   Pin-Priority: 100
   EOF
   ```
2. Add the Bookworm repository signing key:
   ```bash
   sudo mkdir -p /etc/apt/keyrings &&
   curl -fsSL https://ftp-master.debian.org/keys/archive-key-12.asc | gpg --dearmor | sudo tee /etc/apt/keyrings/debian-bookworm.gpg > /dev/null &&
   echo "deb [signed-by=/etc/apt/keyrings/debian-bookworm.gpg] http://deb.debian.org/debian bookworm main" | sudo tee /etc/apt/sources.list.d/debian-bookworm.list &&
   sudo apt update
   ```
3. Add the Ubuntu repositories for AMD64 packages:
   ```bash
   sudo tee /etc/apt/sources.list.d/ubuntu-amd64.list > /dev/null <<EOF
   deb [arch=amd64] http://archive.ubuntu.com/ubuntu noble main restricted universe multiverse
   deb [arch=amd64] http://archive.ubuntu.com/ubuntu noble-updates main restricted universe multiverse
   deb [arch=amd64] http://archive.ubuntu.com/ubuntu noble-security main restricted universe multiverse
   deb [arch=amd64] http://archive.ubuntu.com/ubuntu noble-backports main restricted universe multiverse
   EOF
   ```
4. Update APT and confirm that Wine 8.0 is available. The output should include `8.0~repack-4`:
   ```bash
   sudo apt update
   apt-cache madison wine | grep 8.0
   ```
5. Continue with [step 6 of the Ubuntu/Kali instructions](#ubuntuarm64).
