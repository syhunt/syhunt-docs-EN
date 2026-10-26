# Installing on Linux

> **Applies to:** Syhunt Hybrid 7.2

Syhunt Hybrid runs on Ubuntu (Server or Desktop) and most 64-bit Linux distributions with almost no setup effort, and nearly out of the box on Kali Linux. The Linux edition (*Carbon*) is a Wine-powered installation. For a feature comparison with the Windows edition, see [Installation Overview](install-windows.md#versions).

<a id="dists_begin"></a>

## Overview

Installing Syhunt on Linux always follows the same pattern:

1. Read and accept the [Syhunt EULA](https://www.syhunt.com/en/?n=Legal.EULA) before running any of the commands below.
2. Install **Java** (needed by the setup program), if it isn't already installed.
3. *(Optional)* Install **Git** if you plan to scan Git repositories.
4. Install **Wine64**, following the [Wine guide](wine.md) for your distribution.
5. [Run the Syhunt setup](#finishinstall).

The sections below give the exact commands for each distribution.

<a id="dists_end"></a>

**Distributions:** [Kali](#kali) · [Parrot OS](#parrot) · [Ubuntu](#ubuntu) · [Debian](#debian) · [Fedora](#fedora) · [CentOS / RHEL](#centos) · [AlmaLinux](#almalinux) · [Rocky Linux](#rockylinux) · [Oracle Linux](#oraclelinux) · [Amazon Linux 2](#amazonlinux2) · [Amazon Linux 2023](#amazonlinux2023) · [Arch / Manjaro](#archlinux) · [Linux Mint](#mint) · [MX Linux](#mxlinux) · [openSUSE](#opensuse) · [elementary OS](#elementaryos) · [Deepin](#deepin) · [Solus](#solus)

<a id="kali"></a>

## Kali Linux

> **Applies to:** Kali Linux 2024.3 or later

Kali already includes most of Syhunt's dependencies, so you only need Wine64 and Syhunt itself.

1. Install Wine64 by following the [Debian/Kali Wine guide](wine.md#debian).
2. [Run the Syhunt setup](#finishinstall).

<a id="parrot"></a>

## Parrot OS

> **Applies to:** Parrot OS 6.1 or later

Like Kali, Parrot OS already includes most of Syhunt's dependencies.

1. Install Wine64 by following the [Debian Wine guide](wine.md#debian).
2. [Run the Syhunt setup](#finishinstall).

<a id="ubuntu"></a>

## Ubuntu

> **Applies to:** Ubuntu Desktop/Server 18.10 to 24.04 and later; KDE neon 2020.03 or later

1. Install Java:
   ```bash
   sudo apt install openjdk-17-jre
   ```
2. *(Optional)* Install Git:
   ```bash
   sudo apt install git
   ```
3. Install Wine64 by following the [Ubuntu Wine guide](wine.md#ubuntu).
4. [Run the Syhunt setup](#finishinstall).

<a id="debian"></a>

## Debian

> **Applies to:** Debian 9, 10 or 12 (GUI or minimal)

1. Install Wine64 by following the [Debian Wine guide](wine.md#debian).
2. Install Java (on Debian 9 it is probably already installed):
   ```bash
   sudo apt install default-jre
   ```
3. *(Optional)* Install Git:
   ```bash
   sudo apt install git
   ```
4. [Run the Syhunt setup](#finishinstall).

<a id="fedora"></a>

## Fedora

> **Applies to:** Fedora 32, 39, 40 and 41

1. Apply all pending updates and reboot.
2. Install Java:
   ```bash
   sudo yum install java
   ```
3. *(Optional)* Install Git:
   ```bash
   sudo yum install git
   ```
4. Install Wine64 by following the [Fedora Wine guide](wine.md#fedora).
5. [Run the Syhunt setup](#finishinstall).

> [!TIP]
> If OpenJDK 17 is installed and the setup fails with an error such as
> `Exception in thread "main" java.lang.UnsatisfiedLinkError: Can't load library: /usr/lib/jvm/java-17-openjdk-…/lib/libawt_xawt.so`,
> install the missing OpenJDK packages and run the setup again:
> ```bash
> sudo dnf install java-17-openjdk java-17-openjdk-devel java-17-openjdk-headless
> ```

<a id="centos"></a>
<a id="redhat"></a>

## CentOS and Red Hat Enterprise Linux

> **Applies to:** CentOS 7 or 8 (Everything or Minimal); Red Hat Enterprise Linux

1. Install Wine64 by following the [CentOS Wine guide](wine.md#centos).
2. Install Java:
   ```bash
   sudo yum -y update
   sudo yum install java    # or: sudo yum install java-1.8.0-openjdk
   ```
3. *(Optional; needed on Minimal for Git scans)* Install Git:
   ```bash
   sudo yum install git
   ```
4. *(Minimal installs)* Install wget:
   ```bash
   sudo yum install wget
   ```
5. [Run the Syhunt setup](#finishinstall).

<a id="almalinux"></a>
<a id="rockylinux"></a>
<a id="oraclelinux"></a>

## AlmaLinux, Rocky Linux and Oracle Linux

> **Applies to:** AlmaLinux 9, Rocky Linux 9, Oracle Linux 9

1. Install Wine64 by following the Wine guide for your distribution: [AlmaLinux](wine.md#almalinux), [Rocky Linux](wine.md#rockylinux) or [Oracle Linux](wine.md#oraclelinux).
2. Install Java:
   ```bash
   sudo yum -y update
   sudo yum install java    # or: sudo yum install java-1.8.0-openjdk
   ```
3. *(Optional)* Install Git:
   ```bash
   sudo yum install git
   ```
4. *(AlmaLinux and Rocky Linux)* Install wget:
   ```bash
   sudo yum install wget
   ```
5. Allow unconfined processes to run files that need the SELinux `execmod` permission. This resolves a conflict with Wine; it weakens SELinux slightly but does not disable it.
   ```bash
   sudo setsebool -P selinuxuser_execmod on
   ```
6. [Run the Syhunt setup](#finishinstall).

<a id="amazonlinux2"></a>

## Amazon Linux 2

1. Install Wine64 by following the [Amazon Linux 2 Wine guide](wine.md#amazonlinux2).
2. Install Java:
   ```bash
   sudo yum -y update
   sudo yum install java
   ```
3. *(Optional)* Install Git:
   ```bash
   sudo yum install git
   ```
4. [Run the Syhunt setup](#finishinstall).

<a id="amazonlinux2023"></a>

## Amazon Linux 2023

1. Install Wine64 by following the [Amazon Linux 2023 Wine guide](wine.md#amazonlinux2023).
2. Install Java:
   ```bash
   sudo yum -y update
   sudo yum install java
   ```
3. *(Optional)* Install Git:
   ```bash
   sudo yum install git
   ```
4. Allow the SELinux `execmod` permission, as explained in the [AlmaLinux section](#almalinux):
   ```bash
   sudo setsebool -P selinuxuser_execmod on
   ```
5. [Run the Syhunt setup](#finishinstall).

<a id="archlinux"></a>
<a id="manjaro"></a>

## Arch Linux and Manjaro

> **Applies to:** Arch Linux 2019 or later, Manjaro 19

1. Refresh the pacman database:
   ```bash
   sudo pacman -Syy
   ```
2. Install Java 8:
   ```bash
   sudo pacman -S jre8-openjdk
   ```
3. *(Optional)* Install Git:
   ```bash
   sudo pacman -S git
   ```
4. Install Wine64:
   - **Arch Linux:** follow the [Arch Linux Wine guide](wine.md#archlinux).
   - **Manjaro:** run `sudo pacman -S wine`, then `wine64 ipconfig`. Wine starts and configures itself; press **I** if prompted to install missing components.
5. [Run the Syhunt setup](#finishinstall).

<a id="mint"></a>

## Linux Mint

> **Applies to:** Linux Mint 18, 19 and 20

1. Java is preinstalled on Linux Mint.
2. *(Optional)* Install Git:
   ```bash
   sudo apt install git
   ```
3. Install Wine64 by following the [Linux Mint Wine guide](wine.md#mint).
4. [Run the Syhunt setup](#finishinstall).

<a id="mxlinux"></a>

## MX Linux

> **Applies to:** MX Linux 19.1 or later

1. Java and Git are preinstalled on MX Linux 19.
2. Install Wine64, then run it once so it configures itself:
   ```bash
   sudo apt install wine
   wine64 ipconfig
   ```
3. [Run the Syhunt setup](#finishinstall).

<a id="opensuse"></a>

## openSUSE

> **Applies to:** openSUSE Leap 15, Leap 42 or later, Tumbleweed

1. *(Optional)* Install Git:
   ```bash
   zypper in git        # or, if not found: zypper in git-core
   ```
2. Install Wine64 by following the [openSUSE Wine guide](wine.md#opensuse).
3. [Run the Syhunt setup](#finishinstall).

<a id="elementaryos"></a>

## elementary OS

> **Applies to:** elementary OS 5.0 and 5.1

- **elementary OS 5.1:** follow the [Ubuntu steps](#ubuntu). You may be asked to reboot after installing Wine64.
- **elementary OS 5.0:** read [this warning](wine.md#elementaryos) first. If you still want to proceed, you do so at your own risk.

<a id="deepin"></a>

## Deepin

> **Applies to:** Deepin 15.9

1. Install Java:
   ```bash
   sudo apt install default-jre
   ```
2. *(Optional)* Install Git:
   ```bash
   sudo apt install git
   ```
3. Install Wine64, then run it once so it configures itself:
   ```bash
   sudo apt install wine
   wine64 ipconfig
   ```
4. [Run the Syhunt setup](#finishinstall).

<a id="solus"></a>

## Solus

> **Applies to:** Solus 4.1

1. Install Java (the `java` command will be at `/usr/lib64/openjdk-8/bin/java` or `/usr/lib64/openjdk-11/bin/java`):
   ```bash
   sudo eopkg install openjdk-8    # or: sudo eopkg install openjdk-11
   ```
2. *(Optional)* Install Git:
   ```bash
   sudo eopkg install git
   ```
3. Install Wine64 by following the [Solus Wine guide](wine.md#solus).
4. [Run the Syhunt setup](#finishinstall).

<a id="finishinstall"></a>

## Running the setup (Linux and macOS)

1. Download the Syhunt Hybrid setup with a browser, `wget` or `curl`.
2. Run the setup:

   | Edition | Command |
   |---|---|
   | Hybrid | `java -jar syhunt-hybrid-<version>.jar` |
   | Hybrid on a headless server | `java -jar syhunt-hybrid-<version>.jar -console` |
   | Community | `java -jar <filename>` |

   On Linux, if `binfmt-support` is installed (`sudo apt install binfmt-support`), you can instead make the file executable (`chmod a+rx <file>.jar`) and run it directly, for example `./syhunt-hybrid-<version>.jar` (add `-console` on a headless server).
3. Read the EULA carefully before accepting it, then complete the installer (*Next → Next → Finish*). Syhunt is installed by default in:
   - Linux: `/home/<user>/syhunt-hybrid` or `/home/<user>/syhunt-community`
   - macOS: `/Applications/Syhunt Hybrid/` or `/Applications/Syhunt Community/`
4. [Enable the Syhunt Web UI](web-ui.md#firststeps).

<a id="finishconsole"></a>

### Console mode

If the setup runs in console mode:

1. Read the EULA and enter **1** to accept it (or **2** to reject it; Syhunt cannot be installed without accepting it).
2. Press **Enter** to use the default installation path.
3. Enter **1** to continue.
4. Enter **Y** to install Syhunt Core.
5. Enter **1** to continue. The message **Console installation done** confirms the installation.

**What's next?** Read the [Quick Start](../getting-started/quick-start.md) and the [integration guides](../README.md#integrations).
