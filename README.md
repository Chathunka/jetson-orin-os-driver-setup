# OS and Driver Setup Procedure for NVIDIA Jetson Orin Nano

## Contents
  - [🛠️ Prerequisites](#️-prerequisites)
  - [📥 Step 1: Download JetPack 5.1.3 SD Card Image](#step-1-download-jetpack-513-sd-card-image)
  - [💾 Step 2: Flash JetPack 5.1.3 to microSD Card](#step-2-flash-jetpack-513-to-microsd-card)
  - [🔄 Step 3: Boot Jetson Orin Nano with JetPack 5.1.3](#step-3-boot-jetson-orin-nano-with-jetpack-513)
  - [🔧 Step 4: Update Firmware (QSPI)](#step-4-update-firmware-qspi)
  - [📥 Step 5: Download JetPack 6.2 SD Card Image](#step-5-download-jetpack-62-sd-card-image)
  - [💾 Step 6: Flash JetPack 6.2 to microSD Card](#step-6-flash-jetpack-62-to-microsd-card)
  - [🔄 Step 7: Boot Jetson Orin Nano with JetPack 6.2](#step-7-boot-jetson-orin-nano-with-jetpack-62)
  - [🚀 Step 8: Enable MAXN SUPER Performance Mode](#step-8-enable-maxn-super-performance-mode)
  - [🧪 Step 9: Verify Performance Mode](#step-9-verify-performance-mode)
  - [🧰 Optional: Install NVMe SSD for Enhanced Performance](#optional-install-nvme-ssd-for-enhanced-performance)

> ⚠️ **Note:** This guide outlines the essential steps to upgrade the Jetson Orin Nano Developer Kit from JetPack 5.x to JetPack 6.x.  
> This guide is based on the official instructions provided by NVIDIA. For the most up-to-date information, always refer to the original documentation:  
> [Official Setup Guide – Jetson AI Lab](https://developer.nvidia.com/embedded/jetpack)

---

### 🛠️ Prerequisites

Ensure you have the following before proceeding:

**Hardware:**

- Jetson Orin Nano Developer Kit
- microSD card (64GB or larger)
- (Optional) NVMe SSD for enhanced performance
- USB keyboard and mouse
- Display Port monitor
- 12V DC power supply

**Software:**

- Host PC (Windows) with internet access
- Balena Etcher (for flashing SD card on Windows)

---

### 📥 Step 1: Download JetPack 5.1.3 SD Card Image

1. On your Windows PC, visit the NVIDIA JetPack SDK page.
2. Download the JetPack 5.1.3 SD card image for the Jetson Orin Nano Developer Kit.

### 💾 Step 2: Flash JetPack 5.1.3 to microSD Card

1. Insert the microSD card into your Windows PC.
2. Open Balena Etcher.
3. Select the downloaded JetPack 5.1.3 image file.
4. Choose the correct microSD card as the target.
5. Click "Flash" to write the image onto the card.

### 🔄 Step 3: Boot Jetson Orin Nano with JetPack 5.1.3

1. Insert the flashed microSD card into the Jetson Orin Nano.
2. Connect the keyboard, mouse, and monitor.
3. Power on the device.
4. Complete the initial setup prompts (language, keyboard layout, etc.).

### 🔧 Step 4: Update Firmware (QSPI)

1. Open a terminal on the Jetson Orin Nano.
2. Run the following command to schedule a firmware update:

```bash
sudo /opt/nvidia/l4t-bootloader-config/nv-l4t-bootloader-config.sh
sudo reboot
```

3. Upon reboot, the firmware update will start automatically. Wait for it to complete.

### 📥 Step 5: Download JetPack 6.2 SD Card Image

1. On your Windows PC, go to the NVIDIA JetPack SDK page.
2. Download the JetPack 6.2 SD card image for the Jetson Orin Nano Developer Kit.

### 💾 Step 6: Flash JetPack 6.2 to microSD Card

1. Insert a new microSD card (or reuse the previous one after backing up any important data) into your host PC.
2. Open Balena Etcher.
3. Select the downloaded JetPack 6.2 image.
4. Choose the correct microSD card as the target.
5. Click "Flash" to write the image to the card.

### 🔄 Step 7: Boot Jetson Orin Nano with JetPack 6.2

1. Insert the JetPack 6.2 microSD card into the Jetson Orin Nano.
2. Power on the device and complete the initial setup prompts.
3. Open a terminal and verify the JetPack version:

```bash
cat /etc/nv_tegra_release
```

You should see output indicating JetPack 6.2 and L4T 36.4.3.

4. Schedule a firmware update:

```bash
sudo /opt/nvidia/l4t-bootloader-config/nv-l4t-bootloader-config.sh
sudo reboot
```

5. Wait for the update process to complete after reboot.

### 🚀 Step 8: Enable MAXN SUPER Performance Mode

1. Open a terminal on the Jetson Orin Nano.
2. Remove the existing power mode configuration:

```bash
sudo rm -rf /etc/nvpmodel.conf
```

3. Switch to MAXN SUPER mode:

```bash
sudo nvpmodel -m 2
```

This mode enables the highest performance available.

### 🧪 Step 9: Verify Performance Mode

By default, JetPack 6.2 runs in 25W power mode. To switch to MAXN SUPER mode:

1. Click the NVIDIA icon in the top-right of the Ubuntu desktop panel.
2. Select Power Mode from the dropdown menu.
3. Choose MAXN SUPER to activate maximum performance.

### 🧰 Optional: Install NVMe SSD for Enhanced Performance

#### 🔌 Physical Installation

1. Power off the Jetson developer kit and unplug all peripherals.
2. Insert the NVMe SSD into the M.2 slot on the carrier board. Ensure it is properly seated and secured with a screw.
3. Reconnect the peripherals and power supply.
4. Power on the Jetson developer kit.
5. Once the system boots, verify that the SSD is recognized:

```bash
lspci
```

Example expected output:

```
0007:01:00.0 Non-Volatile memory controller: Marvell Technology Group Ltd. Device 1322 (rev 02)
```

#### 🧾 Format and Mount the SSD

1. Identify the SSD device name:

```bash
lsblk
```

Example output:

```
nvme0n1    259:0    0 238.5G  0 disk
```

2. Format the SSD:

```bash
sudo mkfs.ext4 /dev/nvme0n1
```

3. Create a mount point:

```bash
sudo mkdir /ssd
```

4. Mount the SSD:

```bash
sudo mount /dev/nvme0n1 /ssd
```

#### 🔁 Make the Mount Persistent

1. Find the UUID:

```bash
lsblk -f
```

2. Edit `/etc/fstab` and add the following line:

```bash
UUID=<your-uuid> /ssd ext4 defaults 0 2
```

> Replace `<your-uuid>` with the actual UUID.

---
