# **Gemtek-W1700K - OpenWrt SnapShot Build Script**

A huge thanks to everyone on the [Quantum Fiber W1700k](https://forum.openwrt.org/t/quantum-fiber-w1700k-support/222776) forum for the incredible work that's gone into getting this device to where it is today. Special thanks to [rchen14b](https://github.com/rchen14b) for the fantastic apps included in this build, along with many of your patches.

This is a customized OpenWrt firmware build srript for the **Gemtek W1700K** WiFi 7 (BE19000) router, based on the Airoha AN7581 SoC with MT7996 wireless chipset.

## Build Commands

### Prerequisites (Ubuntu 24.04+)
```bash
sudo apt update
sudo apt install build-essential clang flex bison g++ gawk gcc-multilib \
g++-multilib gettext git libncurses5-dev libssl-dev python3-distutils rsync \ 
unzip zlib1g-dev file wget dos2unix`
```

### Clone Repo & Initial Build

```csharp
git clone https://github.com/Gilly1970/
```
```csharp
sudo chmod 775 -R Gemtek-W1700K
```
### Make the script executable and run script

```bash
# Make script executable
cd Gemtek-W1700K
sudo chmod +x Openwrt_Gemtek_w1700k.sh

# Run script
./Openwrt_Gemtek_w1700k.sh

```

### OpenWrt Build Commands (run inside `openwrt/`)
```bash
make menuconfig              # Configure packages and kernel options interactively
make -j$(nproc)              # Build firmware (parallel)
make -j1 V=s                 # Build with verbose output (for debugging)
./scripts/feeds update -a    # Update package feed definitions
./scripts/feeds install -a   # Install feed package symlinks
make clean                   # Clean build artifacts (keep toolchain)
make dirclean                # Full clean including toolchain
```

### Build Output
`openwrt/bin/targets/airoha/an7581/` — openwrt-airoha-an7581-gemtek_w1700k-ubi-squashfs-sysupgrade.itb/images for flashing

## How the Build Script Works

`Openwrt_Gemtek_w1700k.sh` orchestrates the full setup:
1. Clones OpenWrt from `https://git.openwrt.org/openwrt/openwrt.git`
2. Copies files from `openwrt-patches/` to their destinations in the OpenWrt tree, using `openwrt-patches/openwrt-add-patch` as the mapping list (format: `source_filename:destination_path` for conflicts)
3. Removes files listed in `openwrt-patches/openwrt-remove`
4. Copies `files/` directory into the OpenWrt `files/` overlay (runtime configs)
5. Applies `config/config.diff` as the `.config` build configuration
6. Runs `feeds update/install` then optionally `make menuconfig` before building

**To add a new patch**: place the file in `openwrt-patches/` and add its destination path to `openwrt-patches/openwrt-add-patch`.

**To add a runtime file** (lands on the router filesystem): place it under `files/etc/...`.
