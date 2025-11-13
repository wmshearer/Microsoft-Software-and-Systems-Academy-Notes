# Partition Basics
- A disk must be partitioned before it can be used.
- Partitions are separate physical and logical areas of a disk.
- Each partition can have its own file system and drive letter.
- Different operating systems can exist on different partitions.

# MBR (Master Boot Record)
- Older partition table standard.
- Partition table stored in the first sector of the disk.
- If the first sector is damaged, partitions can become inaccessible.
- Maximum of 4 primary partitions.
- Maximum disk size: 2 TB.
- Primary partitions can be made bootable.
- Extended partitions can contain multiple logical drives.

# Example: Check disk partition style
```powershell
Get-Disk | Select-Object Number, PartitionStyle
```

# GPT (GUID Partition Table)
- Newer modern standard.
- Partition table is stored in two locations for fault tolerance.
- Supports up to 128 partitions.
- Maximum disk size: 18 PB.
- Primary partitions can be bootable.
- Required for modern UEFI-based systems.

# Example: Convert an empty disk to GPT
```powershell
Initialize-Disk -Number 1 -PartitionStyle GPT
```

# Quick Comparison Summary
- MBR = old, limited, single point of failure, 2 TB max.
- GPT = modern, redundant, supports more partitions and massive disk sizes.

# Example: Create a GPT partition and format it
```powershell
New-Partition -DiskNumber 1 -UseMaximumSize -DriveLetter E |
    Format-Volume -FileSystem NTFS -NewFileSystemLabel "Data"
```

# MBR vs GPT Layout (Conceptual)

```powershell
MBR Disk
---------
[ MBR (sector 0) ]
[ Partition 1 ]
[ Partition 2 ]
[ Partition 3 ]
[ Partition 4 or Extended Partition → Logical Drives ]
```
Notes:
- Single point of failure (MBR stored once)
- Max 4 primary partitions
- Max size: 2 TB

```powershell
GPT Disk
---------
[ Protective MBR ]
[ GPT Header (Primary) ]
[ Partition Entries (Primary) ]
[ Partitions (up to 128) ]
[ GPT Header (Backup) ]
[ Partition Entries (Backup) ]
```
Notes:
- Partition tables stored twice (front + back)
- Up to 128 partitions
- Max size: 18 PB

# One-Sentence Memory Trick
```powershell
# Memory Trick
# "MBR = Mini, Basic, Restricted; GPT = Giant, Protected, Tons of partitions."
```

# Quick Comparison Table
```powershell
# MBR vs GPT Comparison Table

Feature                  MBR                          GPT
-------                  ---                          ---
Max Disk Size            2 TB                         18 PB
Max Partitions           4 primary                    128 partitions
Boot Method              BIOS                         UEFI
Fault Tolerance          None (stored once)           Yes (stored twice)
Best Use Case            Legacy systems               Modern disks & OS installs
```