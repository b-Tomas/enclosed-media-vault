# Software

## Current setup: OpenMediaVault

The NAS runs [OpenMediaVault](https://www.openmediavault.org/) on a Raspberry Pi 5, booting from the NVMe SSD behind the Geekworm X1004's PCIe switch.

<!-- TODO: OMV version, share configuration, ZFS setup, apps -->

## SATA controller boot fix

With the ASM1166 SATA controller and hard drives connected, the Pi would crash at boot with:

```
AHCI controller unavailable!
```

The fix is included in recent kernels. Add to `/boot/firmware/config.txt`:

```ini
# Required for SATA controllers behind PCIe switches.
# See: https://github.com/raspberrypi/linux/issues/4848
#      https://forums.raspberrypi.com/viewtopic.php?t=363682#p2195153
dtoverlay=pciex1-compat-pi5,no-mip
dtoverlay=pcie-32bit-dma-pi5
```

## Other OSes explored

### Umbrel

Tried early on. Nice UI, but too limited for NAS use. Lacks proper storage management, RAID, and file sharing configuration.

### NixOS

No official support for RPi 5. I managed to install and boot it but did not invest too much time in setting it up. Built the image on a Pop!\_OS-based x86 VM with Nix installed.

Resources:

- [NixOS on ARM: Raspberry Pi 5](https://wiki.nixos.org/wiki/NixOS_on_ARM/Raspberry_Pi_5)
- [pdg137/pi5-nixos-setup](https://github.com/pdg137/pi5-nixos-setup) (deployment example)
- [nvmd/nixos-raspberrypi](https://github.com/nvmd/nixos-raspberrypi) (upstream reference)

<!-- TODO: NixOS configuration details if/when completed -->
