# Descript Breakdowns

## 1.2

Field           |Example                   |Description
1. Device       |UUID=... or /dev/sdb1     |The identifier for the partition. UUID is preferred as device names can change.
2. Mount Point  |/mnt/data                 |"The directory where the disk will ""live."""
3. FS Type      | ext4                     | "The filesystem (ext4, xfs, btrfs, nfs, etc.)."
4. Options      | defaults                 | "Common flags: rw (read-write), noexec (prevent binaries from running), _netdev (wait for network)."
5. Dump         | 0                        | Legacy backup flag. Almost always 0 today.
6. Pass         | 2                        | "Fsck check order: 1 for root (/), 2 for other drives, 0 to skip check."

## LVM Step-by-Step (The "Building" Process)

LVM is like LEGOs for storage. You take small pieces and build a big structure.

### Step 1: Initialize Physical Volumes (PV)
Tell Linux to treat raw disks/partitions as LVM-ready.

      Command: pvcreate /dev/sdb1 /dev/sdc1

      Verification: pvdisplay or pvs

### Step 2: Create a Volume Group (VG)
Pool those physical disks into one big "bucket" of storage.

      Command: vgcreate vg_data /dev/sdb1 /dev/sdc1

      Verification: vgdisplay or vgs

### Step 3: Carve out a Logical Volume (LV)
This is what you actually format and use.

      Command: lvcreate -L 50G -n lv_projects vg_data

      -L: Size (e.g., 50 Gigabytes)

      -n: Name of the volume

      Verification: lvdisplay or lvs

### Step 4: Format and Mount
Now it behaves like a normal disk.

      Command: mkfs.ext4 /dev/vg_data/lv_projects

      Command: mount /dev/vg_data/lv_projects /projects

## 3. Storage Troubleshooting Commands

On the XK0-006, if a user says "I'm out of space," these are your go-to tools:

* df -h: Shows disk space (Human-readable).

* du -sh /folder: Shows how much space a specific directory is using.

* lsblk: Shows the "tree" view of your blocks (partitions, LVM, and mount points).

* blkid: Used to find the UUID of a disk (essential for fstab).

* fsck: Used to repair a corrupted filesystem (must be unmounted first!).

Quick Scenario Practice:
Question: A technician adds a new 1TB drive and wants it to mount automatically at boot to /backup using the XFS filesystem. They want the system to check it for errors after the root drive is checked.

What would the /etc/fstab line look like?

      UUID=xxxx-xxxx /backup xfs defaults 0 2

## Ansible Playbook Anatomy

---
      - name: Configure Apache Web Servers  # Description of the play
        hosts: web_servers                 # The group of servers from your inventory
        become: yes                        # Run as sudo/root

         tasks:
        - name: Install apache2 package  # Description of the specific task
      apt:                           # The module being used (Debian/Ubuntu)
        name: apache2
        state: present               # "present" means install it

    - name: Start the apache service
      service:                       # The module for managing services
        name: apache2
        state: started
        enabled: yes                 # Make sure it starts on boot


      File Path                       |Purpose

      /etc/services,                  |Lists port numbers and their associated protocols (e.g., 80 = HTTP)."

      /etc/sysconfig/network-scripts/ |(Legacy RHEL) Where interface config files live.

      /etc/netplan/                   |(Modern Ubuntu) Where YAML-based network configs live.

      /etc/protocols                  |Lists available internet protocols (TCP, UDP, ICMP)."

