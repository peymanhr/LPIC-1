# Disks as Block Devices

A block device is any device that supports **random access** in fixed-size **chunks**(sectors typically 512 byte or 4 KiB).

**Random access** means you can directly jump to any location on the device and read/write there, without having to go through the data in sequence.

In Linux, disks of different types(HDD, SSD, NVMe, USB, Fibre Channel) are exposed to the kernel as block devices.

The kernel assigns them names under `/dev`. like `/dev/sda`, `/dev/sdb`, `/dev/nvme0n1`.

### Character Devices
Character devices are sequential access devices (like a **tape drive**, or a **network socket**) forces you to read data in order, from beginning to end. If you want data that’s in the middle, you still have to pass through everything before it.

## Storage Systems

### PATA (Parallel ATA)

Also known as **IDE** (Integrated Drive Electronics) is an obsolete technology. It uses a parallel data bus with 40- or 80-wire ribbon cable. Devices in Linux appear as `/dev/hdX`.

* Used in Personal Computers
* Max Devices per Port: 2 (master/slave)
* Maximum Speed: 1 Gbit/s.

### SCSI (Small Computer System Interface)

SCSI is a set of standards for connecting and transferring data between computers and peripheral devices. SCSI defines a command protocol for communicating with storage devices.

Legacy SCSI devices were parallel SCSI.

* Parallel architecture supports multiple devices on a single bus (up to 16 devices)
* Asynchronous operations

The Linux kernel's SCSI subsystem is a modular, layered architecture that supports SCSI and even non SCSI devices. The following storage protocols are handled by SCSI subsystem.

* SAS
* SATA
* USB Mass Storage
* Fibre Channel
* iSCSI
  
Every device handled by SCSI subsystem appears as `/dev/sd?` (sd = SCSI disk) disks are named with alphabets. (e.g, `/dev/sda`, `/dev/sdb`)

#### SCSI Componets in Linux

* **Mid layer**: Translates generic block requests into SCSI commands.
* **Transport drivers**: Implement protocol (libata, usb-storage, iscsi, etc.).
* **Lower Layer**: Talk to specific HBAs (Host Bus Adapters) or controllers.

### SAS (Serial Attached SCSI)

SAS (Serial Attached SCSI) belongs to SCSI family. SAS is a point-to-point serial transport for SCSI commands replaces older parallel SCSI buses. SAS directly carries SCSI commands over a serial link.

* Used in Enterprise
  - Servers
  - RAID arrays
  - High IOPS
  - Dual-port redundancy
  - High reliability
* Max Devices per Port: 8
* Max Speed: 22.5 Gbit/s

### SATA (Serial ATA)

SATA uses the **ATA command set**, but Linux does not talk ATA directly. **libata** translates ATA commands into the SCSI command set. Thus SATA drives are exposed as SCSI devices. It was invented to replace PATA/IDE.

* Used in Desktops and Laptops:
  - Low cost
  - Simple
  - Thinner cables
* Max Devices per Port: 1
* Max Speed: 6 Gbit/s

### USB Storage

USB Mass Storage devices (like flash drives) typically speak **Bulk-Only Transport** with SCSI command set. This is also handled by SCSI mid-layer. The **usb-storage** driver acts as the lower-level driver (LLD), bridging the USB core to the SCSI mid-layer.

### iSCSI (Internet Small Computer Systems Interface)
iSCSI is a transport protocol that enables the transmission of SCSI commands over standard **TCP/IP** networks, allowing block-level storage access across LANs. Linux handles iSCSI with **open-iscsi** software stack.

### Fibre Channel (FC)

Fibre Channel is a network storage protocol often used in SANs. FC is treated as another transport for the SCSI mid-layer. In practice FC is just a way to send SCSI commands over a dedicated network fabric usually optic.

* Used to connect servers to SANs
* Max Devices per Port: Many via SANs fabric
* Max Speed: 64 Gbit/s

#### FCoE (Fibre Channel over Ethernet)

FCoE encapsulates Fibre Channel frames inside Ethernet frames. So instead of a dedicated Fibre Channel switch/fabric, you can run FC over standard Ethernet.

### NVMe (Non-Volatile Memory Express)

NVMe is designed for SSDs and uses PCIe bus (direct CPU link) bypassing entire SCSI stack. It has its own Linux kernel subsystem.

* High-performance Desktops, Servers and Laptops
* Max Speed: 112 Gbit/s

### Virtio-blk

### Loop Devices

