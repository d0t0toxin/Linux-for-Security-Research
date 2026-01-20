* * *

**Block Device Information:** `lsblk`, `blkid`

* * *

* * *

# `lsblk` Command in Linux

The `lsblk` command in Linux is used to list information about all available or specified block devices. It displays details such as device name, size, type, mount points, and more.

* * *

## Syntax

```bash
lsblk [options] [device]
```

- `device`: Optional. Specify a device (e.g., `/dev/sda`).
    
- `options`: Flags to customize the output.
    

* * *

## Commonly Used Options

| Option | Description |
| --- | --- |
| `-a`, `--all` | Show all devices, including empty devices |
| `-f`, `--fs` | Display filesystem info (type, label, UUID, mount point) |
| `-d`, `--nodeps` | Show only the device, not its partitions |
| `-h`, `--help` | Display help message |
| `-i`, `--ascii` | Use ASCII characters for tree visualization |
| `-l`, `--list` | List output in flat format (no tree) |
| `-m`, `--perms` | Show permissions |
| `-n`, `--noheadings` | Remove header row from output |
| `-o`, `--output <list>` | Specify columns to display |
| `-r`, `--raw` | Output raw device info |
| `-t`, `--topology` | Show device topology information |
| `-J`, `--json` | Output in JSON format |

* * *

## Examples

### 1\. Basic Usage

```bash
lsblk
```

Displays all block devices in a tree-like format.

```
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda      8:0    0 232.9G  0 disk
├─sda1   8:1    0  500M  0 part /boot
├─sda2   8:2    0  50G   0 part /
└─sda3   8:3    0 182.4G 0 part /home
sdb      8:16   0 931.5G 0 disk
└─sdb1   8:17   0 931.5G 0 part /mnt/data
```

### 2\. Show Filesystem Info

```bash
lsblk -f
```

```
NAME   FSTYPE LABEL UUID                                 MOUNTPOINT
sda                                                    
├─sda1 vfat   BOOT  1234-ABCD                            /boot
├─sda2 ext4         e7d8f6a1-3f1c-4e2a-9b7a-1b2c3d4e5f6a /
└─sda3 ext4         1a2b3c4d-5e6f-7a8b-9c0d-1e2f3a4b5c6d /home
sdb                                                    
└─sdb1 ntfs   Data  1234567890ABCDEF                     /mnt/data
```

### 3\. List Only Devices (No Partitions)

```bash
lsblk -d
```

```
NAME MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda    8:0    0 232.9G  0 disk
sdb    8:16   0 931.5G  0 disk
```

### 4\. Display Custom Columns

```bash
lsblk -o NAME,SIZE,TYPE,MOUNTPOINT
```

```
NAME   SIZE TYPE MOUNTPOINT
sda    232.9G disk
sda1   500M  part /boot
sda2   50G   part /
sda3   182.4G part /home
sdb    931.5G disk
sdb1   931.5G part /mnt/data
```

### 5\. Output in JSON Format

```bash
lsblk -J
```

```
{
  "blockdevices": [
    {"name": "sda", "size": "232.9G", "type": "disk", "children": [ ... ]},
    {"name": "sdb", "size": "931.5G", "type": "disk", "children": [ ... ]}
  ]
}
```

### 6\. Display Permissions

```bash
lsblk -m
```

Shows owner, group, and permission for each device.

### 7\. Show Topology Information

```bash
lsblk -t
```

Displays I/O alignment, physical block size, and other low-level info.

### 8\. List Devices Without Tree

```bash
lsblk -l
```

Flat listing, useful for scripting.

```
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT
sda      8:0    0 232.9G 0 disk
sda1     8:1    0  500M 0 part /boot
sda2     8:2    0  50G  0 part /
sda3     8:3    0 182.4G 0 part /home
sdb      8:16   0 931.5G 0 disk
sdb1     8:17   0 931.5G 0 part /mnt/data
```

* * *

`lsblk` is a very versatile command and is widely used for identifying storage devices, checking mount points, and getting detailed information about filesystems and device topology.

* * *

* * *

# `blkid` Command in Linux

The `blkid` command in Linux is used to display or find block device attributes such as UUID, filesystem type, and labels. It is commonly used to identify filesystems, mount devices properly, and retrieve unique identifiers.

* * *

## Syntax

```bash
blkid [options] [device]
```

- `device`: Optional. Specify a block device (e.g., `/dev/sda1`).
    
- `options`: Flags to customize the output.
    

* * *

## Commonly Used Options

| Option | Description |
| --- | --- |
| `-c <file>` | Use a cache file instead of the default `/etc/blkid.tab` |
| `-o <output>` | Output format: `device`, `udev`, `export` |
| `-s <tag>` | Show a specific tag (e.g., `UUID`, `TYPE`, `LABEL`) |
| `-p` | Probe the specified devices even if they are not in `/etc/fstab` |
| `-U <uuid>` | Get device name by UUID |
| `-L <label>` | Get device name by label |
| `-h` | Display help message |

* * *

## Examples

### 1\. Display All Devices with Attributes

```bash
blkid
```

```
/dev/sda1: UUID="1234-ABCD" TYPE="vfat" PARTLABEL="EFI" 
/dev/sda2: UUID="e7d8f6a1-3f1c-4e2a-9b7a-1b2c3d4e5f6a" TYPE="ext4" 
/dev/sda3: UUID="1a2b3c4d-5e6f-7a8b-9c0d-1e2f3a4b5c6d" TYPE="ext4" 
/dev/sdb1: UUID="1234567890ABCDEF" TYPE="ntfs" LABEL="Data"
```

### 2\. Show Only UUID of a Device

```bash
blkid -s UUID -o value /dev/sda2
```

```
e7d8f6a1-3f1c-4e2a-9b7a-1b2c3d4e5f6a
```

### 3\. Show Only Filesystem Type

```bash
blkid -s TYPE -o value /dev/sdb1
```

```
ntfs
```

### 4\. Find Device by UUID

```bash
blkid -U 1234-ABCD
```

```
/dev/sda1
```

### 5\. Find Device by Label

```bash
blkid -L Data
```

```
/dev/sdb1
```

### 6\. Probe Devices Explicitly

```bash
blkid -p /dev/sdc1
```

Probes the specified device even if it is not in the cache.

### 7\. Display in Custom Output Format

```bash
blkid -o export /dev/sda2
```

```
DEVNAME=/dev/sda2
UUID=e7d8f6a1-3f1c-4e2a-9b7a-1b2c3d4e5f6a
TYPE=ext4
```

* * *

`blkid` is a powerful command for retrieving and verifying device identifiers, types, and labels, which is critical for mounting, backup scripts, and system configuration.

* * *

* * *

&nbsp;
