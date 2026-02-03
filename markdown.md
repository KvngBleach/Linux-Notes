# Field/Example/Description

      Device/UUID=... or /dev/sdb1/The identifier for the partition. UUID is preferred as device names can change.

      Mount Point /mnt/data "The directory where the disk will ""live."""

      FS Type ext4 "The filesystem (ext4, xfs, btrfs, nfs, etc.)."

      Options defaults "Common flags: rw (read-write), noexec (prevent binaries from running), _netdev (wait for network)."

      Dump 0 Legacy backup flag. Almost always 0 today.

      Pass 2 "Fsck check order: 1 for root (/), 2 for other drives, 0 to skip check."
