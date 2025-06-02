# Personal TrueNAS SCALE Server Setup

A self-hosted NAS solution using TrueNAS SCALE for centralized file storage, secure remote access via Tailscale VPN, automatic backups, and storage expansion.

---

## Hardware Requirements

- Dual Core Processor (e.g., Intel i3 4th Gen or higher)
- 8-16 GB RAM
- 1 Gbps Ethernet (for faster file transfers)
- SSD (for OS installation)
- HDD or additional SSD (for data storage)

---

## Software Requirements

- [TrueNAS SCALE - Latest Stable Version](https://www.truenas.com/download-truenas-community-edition/)
- [Rufus](https://rufus.ie/) - Single OS Bootable USB Creator
- [BalenaEtcher](https://etcher.balena.io/) - Easy ISO flasher
- [Ventoy](https://www.ventoy.net/) - Multi-ISO Bootable USB Tool

---

## Flashing a USB with BalenaEtcher

1. **Download & Install** [BalenaEtcher](https://etcher.balena.io/).
2. **Download** the TrueNAS SCALE ISO from the [official site](https://www.truenas.com/download-truenas-community-edition/).
3. **Insert USB drive** (min 8GB).
4. Open BalenaEtcher:
   - Click **"Flash from file"** -> select ISO
   - Click **"Select target"** -> choose USB
   - Click **"Flash!"**
5. Wait for the flashing to complete. The USB is now bootable.

---

## Booting from USB & Installing TrueNAS

1. Insert the USB into the NAS/PC.
2. Power on and enter the boot menu (keys: `F2`, `F12`, `DEL`, or `ESC`).
3. Select the USB as the boot device.
4. TrueNAS Installer will load.
5. Follow prompts:
   - Select **Install/Upgrade**
   - Choose your **SSD** as OS drive ( will be wiped)
   - Set **root password**
   - Reboot when done and remove USB

---

## First Access to TrueNAS

1. Connect NAS/PC to your router using an **Ethernet cable**.
2. On boot, a **local IP address** will appear (e.g., `192.168.0.102`).
3. Open a browser on a device in the same network. visit the IP.
4. Login:
   - **Username**: `admin`
   - **Password**: `admin` (or the one set during install)

---

## Storage Setup

### Create Pool

1. Go to **Storage > Pools > Add**
2. Select **Create new pool**
3. Name the pool (e.g., `MainStorage`)
4. Add available HDD/SSD disks
5. Choose redundancy level (stripe, mirror, RAIDZ)
6. Click **Create**

### Create Dataset

1. Click your pool name **Add Dataset**
2. Name it (e.g., `Media`, `Documents`)
3. Set options (permissions, share type)
4. Click **Save**

---

## Users and Groups Setup

### Add Users

1. Go to **Credentials > Local Users > Add**
2. Fill in:
   - Username
   - Password
   - Optional home directory

### Add Groups

1. Go to **Credentials > Local Groups > Add**
2. Name the group and assign users

### Set Dataset Permissions

1. Go to **Storage > Pools > Your Dataset**
2. Click **Edit Permissions**
3. Set user/group ownership
4. Adjust permission mode (read/write/execute)

---

## Install & Configure Tailscale VPN

1. Go to **Apps > Available Applications**
2. Search for **Tailscale** -> Click **Install**
3. Set your **Auth Key** from [Tailscale Admin Panel](https://login.tailscale.com/admin/settings/keys)
4. Confirm and deploy

---

## Accessing TrueNAS Remotely via Tailscale

1. Install **Tailscale** on your mobile or laptop
2. Login to the same Tailscale account
3. Once connected, find your NAS device in the network list
4. Use the **Tailscale IP** to access your TrueNAS dashboard remotely
5. Login with your usual admin/user credentials

---

## Optional: Auto Backup Setup

### Periodic Snapshots

1. Go to **Data Protection > Periodic Snapshot Tasks**
2. Click **Add**
3. Select:
   - Dataset (e.g., `MainStorage/Documents`)
   - Schedule (e.g., daily/hourly)
   - Snapshot lifetime
4. Save

### Replication (Local or Remote)

1. Go to **Data Protection > Replication Tasks**
2. Click **Add**
3. Source: your dataset
4. Destination: another pool, disk, or remote NAS
5. Set schedule and enable auto-run

### Cloud Backup (Optional)

1. Go to **System Settings > Cloud Credentials**
2. Add your cloud provider (Google Drive, S3, etc.)
3. Create a **Cloud Sync Task** under **Data Protection**
4. Configure push/pull, paths, and schedule

---

## Expanding Storage

### Add New Drive to Existing Pool

1. Physically install the new HDD/SSD
2. Go to **Storage > Pools > Status**
3. Click **Expand** Select new disk Attach

> Data redundancy (mirror/RAID) recommended if adding critical storage

### Or Create New Pool

1. Go to **Storage > Pools > Add**
2. Select **Create new pool**
3. Use the new drive for a separate pool (e.g., `BackupPool`)

---

## Final Notes

- Regularly check **disk health** under **Storage > Disks** (SMART tests)
- Enable **email alerts** for system failures under **System Settings > Email**
- Use **Tailscale ACLs** to restrict or allow access per user/device
