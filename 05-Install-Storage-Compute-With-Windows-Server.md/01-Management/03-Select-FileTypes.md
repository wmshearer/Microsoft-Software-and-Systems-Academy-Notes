# Implement ReFS — Study Notes

---

## Overview

ReFS (Resilient File System) was introduced in **Windows Server 2012**.

### Key features
- Designed to increase reliability with **proactive error correction**.
- Can detect and fix corruption automatically.
- Supports volumes up to **35 PB**.

### Recommended use cases
- Virtual disks  
- Storage Spaces Direct environments

### Limitations
- Does **not** support file encryption or compression.
- Not supported by OS versions older than:
  - Windows Server 2012  
  - Windows 8.1
- ReFS volumes are not accessible if the disk is moved to a computer running an older OS.

---

# Use VHD and VHDX File Types (1 of 2)

---

## What is a VHD?

A **virtual hard disk (VHD)** is a file that emulates a physical disk.

### Where it can be used
- Attached to a **physical computer**
- Attached to a **virtual machine (VM)**

### Behavior when attached
Once attached, it appears just like a physical disk:

- Can be initialized as **MBR** or **GPT**
- Can be configured as **basic** or **dynamic**
- Can create partitions/volumes and format them with a file system

### When configured
The VHD will appear in **Windows File Explorer**, and you can interact with it like any physical disk.

---

# Use VHD and VHDX File Types (2 of 2)

---

## VHD vs VHDX formats

### .vhd (older format)
- Maximum size: **2 TB**

### .vhdx (newer format)
- Maximum size: **64 TB**
- Provides better **resilience** and error recovery capabilities

### Typical usage
The most common use is attaching a VHD to a VM:

- It appears as a **physical disk inside the VM**

### Additional notes
- You can attach a VHD to a physical machine as well.
- Virtual disk files can be managed using:
  - **Disk Management**
  - **diskpart**

---

# Select a File System — Study Notes

---

## After disk initialization

Once disks are initialized as **MBR** or **GPT**, and configured as **basic** or **dynamic**, the next step is to create partitions or volumes:

- On a **basic disk**, create a **partition**
- On a **dynamic disk**, create a **volume**

### Formatting
Formatting creates the file system on the partition or volume.

---

## What file systems do

File systems create an indexing system so files can be efficiently stored and retrieved.

Some file systems include additional features such as:
- Setting permissions
- Encrypting files
- Compressing files

---

## File systems supported in Windows Server 2022

- **File Allocation Table (FAT)**
- **New Technology File System (NTFS)**
- **Resilient File System (ReFS)**

