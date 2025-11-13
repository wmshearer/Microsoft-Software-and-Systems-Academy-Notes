# Disk Types in Windows Server 2022 — Study Notes

---

## Basic vs Dynamic Disks

Windows Server 2022 supports configuring disks as either **basic** or **dynamic**.

### Basic disks
Basic disks are the industry standard.

- Basic disks contain **partitions**.
- Partition information is stored in **partition tables**.
- **All operating systems** understand and support basic disks.

### Dynamic disks
Dynamic disks are *not* industry standard.

- Dynamic disks contain **volumes**.
- Volume information is stored in a **nonstandard database**.

### Conversion notes
- Converting **basic → dynamic** is *nondestructive* (data preserved).
- Converting **dynamic → basic** is *destructive* (data lost).

---

## Physical Disk Types Supported by Windows Server 2022

### Enhanced Integrated Drive Electronics (EIDE)
An older storage standard.

- Slow performance  
- Limited to **128 GB**

### Serial Advanced Technology Attachment (SATA)
Replaced EIDE.

- Provides a **much faster** interface

### Small Computer System Interface (SCSI)
Faster than SATA.

- Uses controllers that offload processing from the CPU  
- Highly reliable and supports **hot swapping**

### Serial Attached SCSI (SAS)
A faster evolution of SCSI.

- Uses **serial communication** for improved performance

### Solid-State Drive (SSD)
Modern solid-state storage technology.

- No moving parts; data stored in chips  
- **Very fast**, energy efficient, and more resilient than mechanical drives
