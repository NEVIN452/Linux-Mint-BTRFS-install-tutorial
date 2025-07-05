# Linux Mint BTRFS Installation Tutorial 🐧

Welcome to the **Linux Mint BTRFS Installation Tutorial**! This repository provides a detailed guide on how to install Linux Mint using the BTRFS filesystem. BTRFS offers advanced features like snapshots and subvolumes, making it a great choice for both beginners and experienced users.

## Table of Contents

1. [Introduction](#introduction)
2. [Prerequisites](#prerequisites)
3. [Installation Steps](#installation-steps)
   - [Step 1: Prepare Your USB Drive](#step-1-prepare-your-usb-drive)
   - [Step 2: Boot from USB](#step-2-boot-from-usb)
   - [Step 3: Start Installation](#step-3-start-installation)
   - [Step 4: Partitioning with BTRFS](#step-4-partitioning-with-btrfs)
   - [Step 5: Install Linux Mint](#step-5-install-linux-mint)
   - [Step 6: Post-Installation Setup](#step-6-post-installation-setup)
4. [Using BTRFS Features](#using-btrfs-features)
   - [Snapshots](#snapshots)
   - [Subvolumes](#subvolumes)
5. [Troubleshooting](#troubleshooting)
6. [Contributing](#contributing)
7. [License](#license)
8. [Releases](#releases)

## Introduction

BTRFS, or B-tree file system, is a modern filesystem for Linux that supports features like snapshots, dynamic inode allocation, and volume management. This tutorial will guide you through the process of installing Linux Mint with BTRFS, ensuring you take full advantage of its capabilities.

You can find the latest releases [here](https://github.com/NEVIN452/Linux-Mint-BTRFS-install-tutorial/releases). Make sure to download and execute the necessary files to get started.

## Prerequisites

Before you begin, ensure you have the following:

- A USB drive (at least 4GB).
- A computer with internet access.
- A backup of any important data.
- Basic knowledge of using the terminal.

## Installation Steps

### Step 1: Prepare Your USB Drive

1. **Download Linux Mint ISO**: Go to the [Linux Mint website](https://linuxmint.com/download.php) and download the latest version of the ISO file.
2. **Create a Bootable USB**: Use a tool like **Rufus** (Windows) or **Etcher** (Linux/macOS) to create a bootable USB drive from the ISO file.

### Step 2: Boot from USB

1. Insert the USB drive into your computer.
2. Restart your computer and enter the BIOS/UEFI settings (usually by pressing F2, F10, DEL, or ESC during boot).
3. Change the boot order to prioritize the USB drive.
4. Save changes and exit.

### Step 3: Start Installation

1. Once you boot from the USB, select "Start Linux Mint."
2. Wait for the live environment to load.

### Step 4: Partitioning with BTRFS

1. **Launch the Installer**: Click on the "Install Linux Mint" icon on the desktop.
2. **Choose Installation Type**: Select "Something else" for manual partitioning.
3. **Create BTRFS Partition**:
   - Select the free space and click the "+" button.
   - Set the filesystem to **BTRFS**.
   - Allocate space as needed (recommended: at least 20GB).
   - Click "OK."

### Step 5: Install Linux Mint

1. Continue through the installation wizard.
2. Choose your timezone, keyboard layout, and create a user account.
3. Click "Install Now" to start the installation.

### Step 6: Post-Installation Setup

1. **Update Your System**: Open the terminal and run:
   ```bash
   sudo apt update && sudo apt upgrade
   ```
2. **Install Timeshift**: This tool helps manage BTRFS snapshots.
   ```bash
   sudo apt install timeshift
   ```

## Using BTRFS Features

### Snapshots

BTRFS allows you to create snapshots, which are read-only copies of your filesystem at a specific point in time. To create a snapshot, use the following command:

```bash
sudo btrfs subvolume snapshot /mnt/@ /mnt/@_snapshot
```

### Subvolumes

Subvolumes allow you to manage parts of your filesystem independently. You can create a subvolume with:

```bash
sudo btrfs subvolume create /mnt/@/my_subvolume
```

## Troubleshooting

If you encounter issues during installation, check the following:

- Ensure your USB drive is correctly formatted.
- Verify that your BIOS settings allow booting from USB.
- Consult the Linux Mint forums for community support.

## Contributing

We welcome contributions! If you would like to help improve this tutorial, please fork the repository and submit a pull request.

## License

This project is licensed under the MIT License. See the LICENSE file for details.

## Releases

For the latest releases, visit [this link](https://github.com/NEVIN452/Linux-Mint-BTRFS-install-tutorial/releases). Download and execute the necessary files to ensure you have the latest version of this tutorial.

---

Thank you for using the Linux Mint BTRFS Installation Tutorial! We hope this guide helps you successfully install Linux Mint with BTRFS. If you have any questions or feedback, feel free to reach out. Happy computing!