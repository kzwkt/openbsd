# OpenBSD Setup

## Installation

Official guide:

https://www.openbsd.org/faq/faq4.html#Download

OpenBSD 7.9 amd64 installer:

https://cdn.openbsd.org/pub/OpenBSD/7.9/amd64/install79.img

### 1. Create OpenBSD USB installer

From Linux:

```sh
dd if=install79.img of=/dev/sdb bs=1M
```

**Verify `/dev/sdb` is the USB drive before running this.**

### 2. Prepare the target disk from Linux

Because the target disk needs an OpenBSD partition before installation, create the partition from Linux:

```sh
cfdisk
```

Create the partition that will be used by OpenBSD and change its type to:

```text
OpenBSD data
```

Do not format it with a Linux filesystem.

The resulting layout should contain an **OpenBSD area** that the OpenBSD installer can use.

### 3. Boot the OpenBSD USB

Boot from the installation USB.

Choose:

```text
(I)nstall
```

When asked where to install OpenBSD, select the **OpenBSD area/partition created previously from Linux**.

### 4. Install sets

When asked which sets to install:

```text
Install all sets
```

Complete the installation and reboot.

---

# Network Setup

## Android USB tethering

Enable USB tethering on Android and connect the phone.

Check the kernel messages:

```sh
dmesg
```

Look for recent event eg :

```text
urndis0 ...
```

Create:

```sh
/etc/hostname.urndis0
```

with:

```text
inet autoconf
```

Start the interface:

```sh
sh /etc/netstart urndis0
```

Verify:

```sh
ifconfig urndis0
```

---

# Wireless Firmware

With temporary internet access through USB tethering:

```sh
fw_update
```

Reboot:

```sh
reboot
```

After reboot:

```sh
ifconfig
```

Check for:

```text
iwm0
```

---

# Wi-Fi

Create:

```sh
/etc/hostname.iwm0
```

Example:

```text
join "SSID" wpakey "PASSWORD"
inet autoconf
inet6 autoconf
```
or

ifconfig iwm0 nwid SSIDNAME wpakey PASSWORD

Start Wi-Fi:

```sh
sh /etc/netstart iwm0
```

Check:

```sh
ifconfig iwm0
```

---

# Packages

```sh
pkg_add nano xclip fastfetch firefox vulkan-tools intel-media-driver libva-utils zathura-pdf-mupdf mpv nnn
```

## Package purposes

| Package              | Purpose               |
| -------------------- | --------------------- |
| `nano`               | Text editor           |
| `xclip`              | X11 clipboard         |
| `fastfetch`          | System information    |
| `firefox`            | Web browser           |
| `vulkan-tools`       | Vulkan utilities      |
| `intel-media-driver` | Intel media driver    |
| `libva-utils`        | VA-API utilities      |
| `zathura-pdf-mupdf`  | PDF viewer            |
| `mpv`                | Media player          |
| `nnn`                | Terminal file manager |

---

# Useful Commands

Installed packages:

```sh
pkg_info
```

Update packages:

```sh
pkg_add -u
```

Enabled services:

```sh
rcctl ls on
```

Network interfaces:

```sh
ifconfig
```

Kernel/device messages:

```sh
dmesg
```

Firmware:

```sh
fw_update
```

## Service Management

Enable `apmd` with automatic power management:

```sh
rcctl enable apmd
rcctl set apmd flags -A
rcctl start apmd
```

## Amd gpu issue
radeondrm0 at pci1 dev 0 function 0 "ATI Radeon HD 8670M" rev 0x81
drm1 at radeondrm0
radeondrm0: msi
inteldrm0: 1366x768, 32bpp
wsdisplay0 at inteldrm0 mux 1: console (std, vt100 emulation), using wskbd0
radeondrm0: HAINAN
[drm] *ERROR* Unable to locate a BIOS ROM
drm:pid0:radeondrm_attachhook *ERROR* Fatal error during GPU init

doas config -ef /bsd
disable radeondrm
quit
