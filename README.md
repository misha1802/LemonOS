
# LemonOS 0.8


LemonOS is a lightweight and fast custom Linux distribution built on top of the **Linux 6.18 LTS** long-term kernel and the **apk (Alpine Linux)** package manager.

## Features
- **Base**: Alpine Linux (minirootfs)
- **Kernel**: Linux 6.18.54 LTS
- **Package Manager**: `apk` (fully supports Alpine v3.20 repositories)
- **Power Management**: Working native `poweroff -f` and `reboot -f` commands
- **Installer**: Automation script `lemon-install` to format and deploy to a real PC / VirtualBox HDD

## Host Build Dependencies

Before building the ISO, you need to install package generation utilities on your host system:

### Fedora / RHEL
```bash
sudo dnf install xorriso syslinux-nonlinux
```

### Debian / Ubuntu / Mint
```bash
sudo apt update
sudo apt install xorriso syslinux-utils isolinux
```

### Arch Linux / Manjaro
```bash
sudo pacman -Syu xorriso syslinux
```

## How to Build the Bootable ISO

1. Ensure your compiled LTS kernel image (`bzImage`) is copied to the ISO structure:
   ```bash
   cp path/to/bzImage iso/boot/vmlinuz
   ```

2. Locate the system bootloader files (`isolinux.bin` and `ldlinux.c32`) on your host and copy them to `iso/boot/isolinux/`.
   *(Paths might differ based on your host distro: e.g., `/usr/share/syslinux/` on Fedora/Arch or `/usr/lib/syslinux/bios/` on Debian).*

3. Pack the root filesystem and compile the final ISO:
   ```bash
   cd rootfs
   sudo find . -print0 | sudo cpio --null -ov --format=newc | gzip -9 > ../iso/boot/initramfs.cpio.gz
   cd ..
   
   xorrisofs -o LemonOS.iso \
       -b boot/isolinux/isolinux.bin \
       -c boot/isolinux/boot.cat \
       -no-emul-boot -boot-load-size 4 -boot-info-table \
       ./iso
       
   isohybrid LemonOS.iso
   ```

## Installation on target PC / VirtualBox Boot the `LemonOS.iso` inside your virtual machine or on physical hardware. Once you enter the root shell, run the custom installer:
```bash
lemon-install
```
<img width="1035" height="800" alt="Снимок экрана_20261001_222956" src="https://github.com/user-attachments/assets/c3d49664-241d-48b4-928a-3c232fa576a4" />

