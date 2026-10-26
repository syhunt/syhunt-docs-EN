# Installing Wine

Syhunt Hybrid for Linux and macOS (the *Carbon* edition) runs on Wine64. This page explains how to install Wine64 on each supported system. Once Wine is installed, return to the [Linux](install-linux.md) or [macOS](install-macos.md) guide to install Syhunt.

> [!IMPORTANT]
> Read and accept the [Syhunt EULA](https://www.syhunt.com/en/?n=Legal.EULA) before running any of the commands below.

**Systems:** [macOS (Apple Silicon)](#macos) · [macOS (Intel)](#macos_intel) · [Ubuntu](#ubuntu) · [Debian / Kali](#debian) · [Fedora](#fedora) · [CentOS](#centos) · [AlmaLinux](#almalinux) · [Rocky Linux](#rockylinux) · [Oracle Linux](#oraclelinux) · [Amazon Linux 2](#amazonlinux2) · [Amazon Linux 2023](#amazonlinux2023) · [Arch Linux](#archlinux) · [Linux Mint](#mint) · [openSUSE](#opensuse) · [elementary OS](#elementaryos) · [Solus](#solus)

> [!TIP]
> On every system, the last step is to run `wine64 ipconfig` once. Wine starts for the first time and configures itself; if it asks to install missing components (such as Wine Mono or Wine Gecko), press **I** to install them. When it finishes, you'll see a message like `wine: configuration in '/home/<user>/.wine' has been updated`.

<a id="macos"></a>

## macOS (Apple Silicon)

> **Applies to:** macOS Sequoia on Apple Silicon (M1, M2, M3, M4)

1. Install [Homebrew](https://brew.sh), if you haven't already. Open **Terminal** (in `/Applications/Utilities/`) and run:
   ```bash
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
   ```
   Enter your admin password when asked. At the end, Homebrew prints instructions for setting up `~/.zprofile`; follow them.
2. Install XQuartz (you may need to enter your admin password again):
   ```bash
   brew install --cask xquartz
   ```
3. Install Rosetta:
   ```bash
   softwareupdate --install-rosetta --agree-to-license
   ```
4. Install Wine:
   ```bash
   brew install --cask --no-quarantine wine-stable
   ```
   If your Homebrew doesn't support `--no-quarantine`, run these instead:
   ```bash
   brew install --cask wine-stable
   xattr -dr com.apple.quarantine /Applications/Wine\ Stable.app
   ```
5. After the message `wine-stable was successfully installed!`, run `wine64 ipconfig`.

<a id="macos_intel"></a>

## macOS (Intel)

> **Applies to:** macOS Big Sur and Monterey on Intel

1. Install [Homebrew](https://brew.sh), if you haven't already. Open **Terminal** and run:
   - macOS Catalina and later:
     ```bash
     /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
     ```
   - macOS High Sierra, Sierra, El Capitan and earlier:
     ```bash
     /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
     ```

   Enter your admin password when asked, and follow Homebrew's instructions for setting up `~/.zprofile`.
2. Install XQuartz:
   ```bash
   brew install --cask xquartz
   ```
3. Install Wine (see the Apple Silicon section for the alternative if `--no-quarantine` isn't supported):
   ```bash
   brew install --cask --no-quarantine wine-stable
   ```
4. Run `wine64 ipconfig`.

<a id="ubuntu"></a>

## Ubuntu

> **Applies to:** Ubuntu 18.10 to 24.04 and later

```bash
sudo apt install wine-stable
wine ipconfig
```

Syhunt is 64-bit and doesn't need 32-bit Wine. If you want it anyway:

```bash
sudo dpkg --add-architecture i386 && sudo apt-get update
sudo apt-get install wine32
```

<a id="debian"></a>

## Debian and Kali Linux

> **Applies to:** Debian 9, 10 or 12 (GUI or minimal); Kali Linux 2024.3

```bash
sudo apt-get update
sudo apt install wine
wine64 ipconfig
```

<a id="fedora"></a>

## Fedora

> **Applies to:** Fedora 32, 39 and 40

1. Check for updates:
   ```bash
   sudo dnf check-update
   ```
2. Run `wine64`. When asked whether to install `wine-core`, type **yes**, then **Y** to confirm the changes.

   If downloading the Wine packages fails, apply all updates, reboot and try again:
   ```bash
   sudo dnf update
   sudo reboot
   sudo dnf install wine
   ```
3. Run `wine64 ipconfig` and press **I** to install Wine Mono and Wine Gecko when prompted. Wine may print debug errors in the background; you can safely ignore them.

<a id="centos"></a>

## CentOS

> **Applies to:** CentOS 7.7, 7.8, 8.0 and 8.2 (Everything or Minimal)

1. Update the system:
   ```bash
   sudo yum -y update
   ```
   If this fails with **No URLs in mirrorlist** (CentOS 8 has reached end of life), point the repositories at the CentOS vault. DNF will work again, but you won't receive further updates ([details](https://stackoverflow.com/questions/70926799/centos-through-a-vm-no-urls-in-mirrorlist)):
   ```bash
   sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-Linux-*
   sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-Linux-*
   sudo yum -y update
   ```
2. Install EPEL and Wine:
   ```bash
   sudo yum install epel-release
   sudo yum install wine
   ```
   If Wine64 can't be installed because SDL2 is missing, install SDL2 first and then run `sudo yum install wine` again:
   - Enable the repository that provides SDL2:
     ```bash
     sudo dnf config-manager --set-enabled PowerTools
     ```
   - CentOS 8.2.3 and later:
     ```bash
     sudo dnf --enablerepo=PowerTools install SDL2
     ```
   - CentOS 8.4 and later:
     ```bash
     sudo dnf -y update
     sudo dnf groupinstall 'Development Tools' -y
     sudo dnf config-manager --set-enabled powertools
     sudo dnf -y install SDL2-devel
     ```
3. Run `wine64 ipconfig`.

<a id="almalinux"></a>
<a id="rockylinux"></a>

## AlmaLinux and Rocky Linux

> **Applies to:** AlmaLinux 9, Rocky Linux 9

```bash
sudo yum -y update
sudo dnf install yum-utils
sudo yum install epel-release
sudo dnf config-manager --set-enabled crb
sudo dnf install mingw64-zlib
sudo yum install wine
wine64 ipconfig
```

<a id="oraclelinux"></a>

## Oracle Linux

> **Applies to:** Oracle Linux 9

```bash
sudo yum -y update
sudo yum install epel-release
sudo yum install SDL2-devel
sudo yum install wine
```

If Wine64 can't be installed because nothing provides `mingw64-zlib` and `mesa-libOSMesa`, enable the CodeReady Builder repository, install `mingw64-zlib`, then install Wine again:

```bash
sudo dnf config-manager --set-enabled ol9_codeready_builder
sudo dnf install mingw64-zlib
sudo yum install wine
```

Finally, run `wine64 ipconfig`.

<a id="amazonlinux2"></a>

## Amazon Linux 2

```bash
sudo yum update -y
sudo yum groupinstall "Development Tools" -y
sudo amazon-linux-extras install epel -y
```

Install two key Wine dependencies from compatible Oracle Linux packages, then Wine itself:

```bash
sudo yum install -y https://yum.oracle.com/repo/OracleLinux/OL8/appstream/x86_64/getPackage/spirv-tools-libs-2023.1-2.el8.x86_64.rpm
sudo yum install -y https://yum.oracle.com/repo/OracleLinux/OL7/9/base/x86_64/getPackage/isdn4k-utils-3.2-99.el7.x86_64.rpm
sudo yum install -y wine
wine64 ipconfig
```

<a id="amazonlinux2023"></a>

## Amazon Linux 2023

1. Add the repositories Wine needs, using Syhunt's helper script:
   ```bash
   sudo yum groupinstall "Development Tools" -y
   wget https://syhunt.fra1.cdn.digitaloceanspaces.com/tools/linux/al2023-createrepos.sh
   chmod +x al2023-createrepos.sh
   ./al2023-createrepos.sh
   ```
2. Install Wine and run it once:
   ```bash
   sudo yum update -y
   sudo yum install wine -y
   wine64 ipconfig
   ```
3. To prevent conflicts, remove the Oracle Linux 9 repositories added by the script, which are no longer needed:
   ```bash
   sudo rm /etc/yum.repos.d/epel.repo
   sudo rm /etc/yum.repos.d/appstream.repo
   sudo rm /etc/yum.repos.d/crb.repo
   ```

<a id="archlinux"></a>

## Arch Linux

> **Applies to:** Arch Linux 2019

1. Enable multilib support in pacman: edit `/etc/pacman.conf` (for example, `sudo vi /etc/pacman.conf`) and uncomment these lines, then save the file:
   ```ini
   [multilib]
   Include = /etc/pacman.d/mirrorlist
   ```
2. *(Optional)* Refresh your pacman keys:
   ```bash
   sudo pacman-key --init
   sudo pacman-key --populate archlinux
   sudo pacman-key --keyserver keyserver.ubuntu.com --refresh-keys
   sudo pacman -S archlinux-keyring
   ```
3. Refresh the package database and install Wine:
   ```bash
   sudo pacman -Syy
   sudo pacman -S wine
   ```
4. Run `wine64 ipconfig` and press **I** to install Wine Mono and Wine Gecko when asked.

<a id="mint"></a>

## Linux Mint

> **Applies to:** Linux Mint 18, 19 and 20

### Linux Mint 20

```bash
sudo apt-get update
sudo apt install wine-stable
wine64 ipconfig
```

### Linux Mint 18 and 19

1. Enable the 32-bit architecture (required by the packages, even though Syhunt itself is 64-bit) and install Wine:
   ```bash
   sudo dpkg --add-architecture i386
   sudo apt install wine-stable
   ```
2. If the installation fails because of dependency problems (as it did in our tests), install Wine from the WineHQ repository instead:
   ```bash
   wget -nc https://dl.winehq.org/wine-builds/winehq.key; sudo apt-key add winehq.key
   sudo apt-add-repository 'deb https://dl.winehq.org/wine-builds/ubuntu/ bionic main'
   sudo add-apt-repository ppa:cybermax-dexter/sdl2-backport
   sudo apt update && sudo apt install --install-recommends winehq-stable
   ```
   Use `bionic` for Linux Mint 19.x or `xenial` for Linux Mint 18.x. The PPA provides the required `libfaudio0` library.
3. Run `wine64 ipconfig`.

<a id="opensuse"></a>

## openSUSE

> **Applies to:** openSUSE Leap 15, Leap 42 or later, Tumbleweed

1. Add the Wine repository for your openSUSE version. The URL pattern is `https://download.opensuse.org/repositories/Emulators:/Wine/openSUSE_<name>_<version>/Emulators:Wine.repo`. For example:
   ```bash
   # Leap 15.1
   sudo zypper ar -r https://download.opensuse.org/repositories/Emulators:/Wine/openSUSE_Leap_15.1/Emulators:Wine.repo
   # Tumbleweed
   sudo zypper ar -r https://download.opensuse.org/repositories/Emulators:/Wine/openSUSE_Tumbleweed/Emulators:Wine.repo
   ```
2. Refresh the zypper cache. When asked whether to trust the new repository key, enter **a**:
   ```bash
   sudo zypper ref
   ```
3. Install Wine (enter **y** to start the installation) and run it once:
   ```bash
   sudo zypper in wine
   wine64 ipconfig
   ```

<a id="elementaryos"></a>

## elementary OS

> **Applies to:** elementary OS 5.0 and 5.1

- **elementary OS 5.1:** follow the [Ubuntu](#ubuntu) instructions.
- **elementary OS 5.0:** not supported. In our tests, installing Wine left the system unable to boot, so we don't provide instructions for it.

<a id="solus"></a>

## Solus

> **Applies to:** Solus 4.1

> [!WARNING]
> Solus is not supported. Wine64 was unstable on Solus 4.1 in our tests (errors such as `wine client error:6c: write: Bad file descriptor`), with both the `wine-stable` and `wine-devel` packages.

If you still want to try:

```bash
sudo eopkg upgrade openssl
sudo eopkg install wine-devel
wine64 ipconfig
```
