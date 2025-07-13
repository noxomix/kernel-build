# Linux Kernel Build Instructions

## Prerequisites
- Build tools: gcc, make, flex, bison, libssl-dev, libelf-dev
- Sufficient disk space (at least 2GB free)

## Build Steps

### 1. Configure the kernel
First, create a default configuration for x86_64 architecture:
```bash
make x86_64_defconfig
```

### 2. Enable required features
Use menuconfig to enable OverlayFS and VirtIO MMIO support:
```bash
make menuconfig
```

Navigate to the following options and enable them:

#### Enable OverlayFS:
- File systems → Overlay filesystem support (CONFIG_OVERLAY_FS)
- Press 'Y' or 'M' to enable

#### Enable VirtIO MMIO:
- Device Drivers → Virtio drivers → Platform bus driver for memory mapped virtio devices (CONFIG_VIRTIO_MMIO)
- Press 'Y' or 'M' to enable

Save and exit menuconfig when done.

### 3. Build the kernel
Build the kernel with all available CPU cores:
```bash
make -j$(nproc)
```

### 4. Locate the kernel image
After successful compilation, the kernel image will be available at:
```
arch/x86/boot/bzImage   # Compressed kernel image
vmlinux                 # Uncompressed kernel image (use this for VMs)
```

## Notes
- The `vmlinux` file in the root directory is the uncompressed kernel image typically used for virtual machines
- Build time depends on your hardware but typically takes 10-30 minutes
- Make sure you have enough disk space before starting the build