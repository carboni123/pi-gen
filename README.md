# Minimal Raspberry Pi OS Image Builder

This is a customized pi-gen fork that builds a minimal, secure Raspberry Pi OS image for headless deployment.

## What It Does
- Creates a bootable Raspberry Pi OS image with only essential components
- Focuses on security by removing unnecessary packages and services
- Includes SSH and wireless/ethernet networking for remote access
- Designed for Infrastructure as Code (IAC) deployment

## What's Included
- **Core System**: Debian Trixie base, Raspberry Pi firmware, bootloader
- **Networking**: Ethernet, wireless (WiFi), systemd-timesyncd for NTP
- **Security**: SSH server, sudo, ca-certificates, minimal user setup
- **Tools**: curl, rsync, raspberrypi-sys-mods, basic system utilities

## What's Removed
- GUI/desktop components
- Development tools (Python, build-essential, etc.)
- Unnecessary utilities (htop, man-db, unzip, etc.)
- Services like avahi (mDNS), auto-resize, extra user groups
- Cloud-init and other optional features

## Building
Requires Debian-based system with pi-gen dependencies.

```bash
# Install dependencies (run as root)
apt install coreutils quilt parted qemu-user-static debootstrap zerofree zip \
dosfstools e2fsprogs libarchive-tools libcap2-bin grep rsync xz-utils file git curl bc \
gpg pigz xxd arch-test bmap-tools kmod

# Build the minimal image (run as root)
./build.sh
```

Output: `deploy/raspios-minimal-*.img.xz`

**Note**: The build script has been simplified for this minimal setup and only builds the essential stages (stage0→stage1→stage2→export).

## Configuration
Edit `config` for settings like SSH enablement, user credentials, etc.

## Stages
- **Stage 0**: Bootstrap Debian filesystem + firmware
- **Stage 1**: Bootable system + basic networking
- **Stage 2**: Minimal lite system + SSH + wireless (trimmed for security)
- **Stage 3+**: Skipped (no desktop/full features)

## Security Notes
- SSH enabled for headless access
- Root account locked
- Minimal attack surface
- Consider `PUBKEY_ONLY_SSH=1` for key-only authentication
