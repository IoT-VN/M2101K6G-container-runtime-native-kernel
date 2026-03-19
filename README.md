# M2101K6G Container Runtime Native Kernel

Native kernel patch set for running container/chroot-style workloads on **Xiaomi sweet** devices with **LineageOS 23.2**.

---

## Supported models

Your device model must match **exactly** one of these:

- `M2101K6G`
- `M2101K6R`

If your model is different, **do not use this build**.

---

## Base ROM

This setup was tested on:

- `lineage-23.2-20260319-nightly-sweet-signed.zip`
- [lineage-23.2-20260319-nightly-sweet-signed.zip]([https://github.com/ravindu644/Android-Kernel-Tutorials](https://mirrorbits.lineageos.org/full/sweet/20260319/lineage-23.2-20260319-nightly-sweet-signed.zip)
---

## What this kernel patch set does

This patch set keeps normal device hardware support while adding the missing kernel features needed for:

- container runtime support
- chroot/proot-like environments
- namespace isolation
- cgroups
- veth/bridge networking
- overlay filesystem
- seccomp
- netfilter/NAT
- better compatibility with init systems and isolated userspace

In short: this is a **device-ready kernel config with container-oriented native kernel support**.

---

## Root / patched boot image

### Steps

1. Extract or download the correct `boot.img` from - [Releases]([https://github.com/ravindu644/Android-Kernel-Tutorials](https://github.com/IoT-VN/M2101K6G-container-runtime-native-kernel/releases/download/v1.0/boot.img)
2. Open **Magisk** and patch that `boot.img`.
3. Magisk will generate a patched image, usually named:
   - `magisk_patched.img`
4. Reboot the phone into **fastboot** mode.
5. Flash the patched boot image:

```bash
fastboot flash boot magisk_patched.img
```

---

## Release notes

### Device / hardware support kept

The following hardware-related configs are enabled or preserved:

- **Camera**
- **Compass**
- **Fingerprint**
- **Haptics**
- **IR**
- **MMC / storage stack**
- **NFC**
- **Power / charging**
- **Step motor**
- **Touchscreen**
- **Ultrasound proximity**

### Critical container/runtime fixes added

The important runtime-oriented additions include:

- **Namespaces** (`PID`, `UTS`, `IPC`, `USER`, `MNT`, `NET`)
- **Cgroups** and process/resource accounting
- **OverlayFS** and `FUSE`
- **Seccomp** and syscall filtering
- **VETH** and **Bridge** networking
- **Netfilter / NAT / MASQUERADE / TCPMSS**
- **Conntrack** and IPv4 NAT compatibility
- **devtmpfs / devpts / tmpfs / proc / sysfs**
- **BPF / tun / packet / unix sockets**
- **IPC features** required by isolated userspace tools

---

## Patched config summary

### General / device

```config
CONFIG_ANDROID_PARANOID_NETWORK=n
CONFIG_MACH_XIAOMI_SWEET=y
CONFIG_CMDLINE="cgroup_disable=pressure ramoops_memreserve=4M"
```

### Camera

```config
CONFIG_LDO_WL2866D=y
CONFIG_SPECTRA_CAMERA_UPGRADE=y
```

### Compass

```config
CONFIG_AKM09970=y
```

### Fingerprint

```config
CONFIG_INPUT_FINGERPRINT=y
CONFIG_FINGERPRINT_FPC_TEE=y
CONFIG_FINGERPRINT_FS_TEE=y
```

### Haptics

```config
CONFIG_INPUT_AW8624_HAPTIC=y
# CONFIG_LEDS_QPNP_HAPTICS is not set
# CONFIG_LEDS_QPNP_VIBRATOR_LDO is not set
```

### IR

```config
CONFIG_RC_CORE=y
CONFIG_LIRC=y
CONFIG_RC_DEVICES=y
CONFIG_IR_SPI=y
```

### MMC / storage

```config
CONFIG_MMC=y
CONFIG_MMC_PERF_PROFILING=y
CONFIG_MMC_BLOCK_MINORS=32
CONFIG_MMC_BLOCK_DEFERRED_RESUME=y
CONFIG_MMC_TEST=m
CONFIG_MMC_PARANOID_SD_INIT=y
CONFIG_MMC_CLKGATE=y
CONFIG_MMC_SDHCI=y
CONFIG_MMC_SDHCI_PLTFM=y
CONFIG_MMC_SDHCI_MSM=y
CONFIG_MMC_CQ_HCI=y
CONFIG_MMC_CQ_HCI_CRYPTO=y
CONFIG_MMC_CQ_HCI_CRYPTO_QTI=y
```

### NFC

```config
CONFIG_NFC_NQ_PN80T=y
# CONFIG_NFC_NQ is not set
```

### Power / charging

```config
CONFIG_BATT_VERIFY_BY_DS28E16=y
CONFIG_BQ2597X_CHARGE_PUMP=y
CONFIG_K6_CHARGE=y
CONFIG_ONEWIRE_GPIO=y
# CONFIG_SMB1355_SLAVE_CHARGER is not set
# CONFIG_SMB1390_CHARGE_PUMP_PSY is not set
```

### Step motor

```config
CONFIG_TI_DRV8846=y
```

### Touchscreen

```config
CONFIG_TOUCHSCREEN_GOODIX_GTX9896_I2C=y
CONFIG_TOUCHSCREEN_FTS_K6=y
CONFIG_TOUCHSCREEN_XIAOMI_TOUCHFEATURE=y
CONFIG_NEW_TOUCH_GAMEMODE=y
CONFIG_TOUCHSCREEN_TDDI_DBCLK=y
```

### Ultrasound proximity

```config
CONFIG_US_PROXIMITY=y
```

---

## Critical runtime patch block

### Namespaces

```config
CONFIG_NAMESPACES=y
CONFIG_PID_NS=y
CONFIG_UTS_NS=y
CONFIG_IPC_NS=y
CONFIG_USER_NS=y
CONFIG_MNT_NS=y
CONFIG_NET_NS=y
```

### Core filesystems

```config
CONFIG_PROC_FS=y
CONFIG_SYSFS=y
CONFIG_TMPFS=y
CONFIG_TMPFS_POSIX_ACL=y
CONFIG_DEVTMPFS=y
CONFIG_DEVTMPFS_MOUNT=y
CONFIG_DEVPTS_FS=y
CONFIG_DEVPTS_MULTIPLE_INSTANCES=y
```

### Cgroups

```config
CONFIG_CGROUPS=y
CONFIG_CGROUP_DEVICE=y
CONFIG_CGROUP_PIDS=y
CONFIG_MEMCG=y
CONFIG_CGROUP_SCHED=y
CONFIG_FAIR_GROUP_SCHED=y
CONFIG_CFS_BANDWIDTH=y
CONFIG_CGROUP_FREEZER=y
CONFIG_CGROUP_NET_PRIO=y
CONFIG_CGROUP_CPUACCT=y
```

### Chroot / userspace compatibility

```config
CONFIG_EVENTFD=y
CONFIG_EPOLL=y
CONFIG_SYSCTL=y
CONFIG_SYSVIPC=y
CONFIG_POSIX_MQUEUE=y
```

### Container networking

```config
CONFIG_VETH=y
CONFIG_BRIDGE=y
CONFIG_TUN=y
CONFIG_PACKET=y
CONFIG_UNIX=y
```

### Security / isolation

```config
CONFIG_SECCOMP=y
CONFIG_SECCOMP_FILTER=y
CONFIG_BPF=y
CONFIG_BPF_SYSCALL=y
```

### Overlay / runtime filesystem

```config
CONFIG_OVERLAY_FS=y
CONFIG_FUSE_FS=y
```

### Firmware loading

```config
CONFIG_FW_LOADER=y
CONFIG_FW_LOADER_USER_HELPER=y
CONFIG_FW_LOADER_COMPRESS=y
```

### Netfilter / NAT / iptables

```config
CONFIG_NETFILTER=y
CONFIG_NETFILTER_ADVANCED=y
CONFIG_IP_NF_FILTER=y
CONFIG_NF_NAT=y
CONFIG_NETFILTER_XT_MATCH_ADDRTYPE=y
CONFIG_IP_NF_TARGET_MASQUERADE=y
CONFIG_NETFILTER_XT_TARGET_MASQUERADE=y
CONFIG_NETFILTER_XT_TARGET_TCPMSS=y
CONFIG_NF_CONNTRACK=y
CONFIG_NF_CONNTRACK_NETLINK=y
CONFIG_NF_NAT_REDIRECT=y
CONFIG_NF_CONNTRACK_IPV4=y
CONFIG_IP_NF_IPTABLES=y
CONFIG_NF_NAT_IPV4=y
CONFIG_IP_NF_NAT=y
CONFIG_IP_ADVANCED_ROUTER=y
CONFIG_IP_MULTIPLE_TABLES=y
CONFIG_NETFILTER_XTABLES=y
```

### Debug / kernel config exposure

```config
CONFIG_IKCONFIG=y
CONFIG_IKCONFIG_PROC=y
```

---

## Notes

- `CONFIG_ANDROID_PARANOID_NETWORK=n` is included to avoid connectivity issues on older kernels.
- `OPENAI_BASE_URL` / userspace tooling notes are unrelated to this kernel patch set.
- This project assumes you already know how to unpack ROM boot images and use fastboot safely.

---

## Warning

Flashing the wrong boot image or using this on an unsupported model may cause:

- bootloop
- broken radio/network
- hardware regressions
- loss of recovery/root state

Double-check:

- device model
- ROM version
- source `boot.img`
- patched output image

before flashing.
