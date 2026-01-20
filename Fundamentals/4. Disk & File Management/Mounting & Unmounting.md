* * *

**Mounting and Unmounting:** `mount`, `umount`

* * *

* * *

# Mounting and Unmounting in Linux

Linux organizes all files and devices into a single **directory tree** starting from `/` (root). Devices like disks, USB drives, CD-ROMs, or network shares are **not automatically part of the filesystem tree** until they are “mounted.” Similarly, when a device is no longer needed, it should be **unmounted** safely.

* * *

## 1\. Mounting in Linux

### What is Mounting?

Mounting is the process of making a storage device or filesystem accessible at a specific point in the directory tree called a **mount point**.

- Example: `/mnt` or `/media/usb`.
    
- After mounting, files on the device appear in the directory like normal files.
    

### Steps to Mount a Device

#### Step 1: Identify the Device

Devices are usually represented as `/dev/sdX` or `/dev/nvmeXnY`.

```bash
lsblk
```

Example output:

| NAME | SIZE | TYPE | MOUNTPOINT |
| --- | --- | --- | --- |
| sda | 500G | disk | /   |
| sdb | 16G | disk |     |

Here, `/dev/sdb` is a USB drive.

#### Step 2: Create a Mount Point

```bash
sudo mkdir -p /mnt/usb
```

#### Step 3: Mount the Device

```bash
sudo mount /dev/sdb1 /mnt/usb
```

- `/dev/sdb1` → device partition
    
- `/mnt/usb` → mount point
    

Verify mount:

```bash
df -h
```

### Mount Options

| Option | Meaning |
| --- | --- |
| `ro` | Read-only |
| `rw` | Read-write (default) |
| `noexec` | Don’t allow executing binaries |
| `nosuid` | Ignore set-user-ID bits |
| `user` | Allow normal users to mount |
| `auto` | Mount automatically at boot |

Example: Mount read-only:

```bash
sudo mount -o ro /dev/sdb1 /mnt/usb
```

### Mounting Remote Filesystems

**NFS (Network File System)**

```bash
sudo mount -t nfs 192.168.1.100:/shared /mnt/nfs
```

**CIFS/SMB (Windows Network Share)**

```bash
sudo mount -t cifs //192.168.1.100/shared /mnt/windows -o username=admin,password=1234
```

* * *

## 2\. Unmounting in Linux

### What is Unmounting?

Unmounting safely disconnects a filesystem from the directory tree, ensuring:

- All pending writes are completed.
    
- No programs are using the filesystem.
    

> Warning: Do **not** remove a USB drive without unmounting to avoid data corruption.

### Unmount Command

```bash
sudo umount <mount_point_or_device>
```

Examples:

```bash
sudo umount /mnt/usb
sudo umount /dev/sdb1
```

### Troubleshooting Unmount

If unmount fails:

```bash
umount: /mnt/usb: target is busy.
```

Check which processes are using the mount:

```bash
lsof /mnt/usb
```

Force unmount (use carefully):

```bash
sudo umount -l /mnt/usb   # lazy unmount
sudo umount -f /mnt/usb   # force unmount
```

* * *

## 3\. Persistent Mounts via `/etc/fstab`

1.  Edit `/etc/fstab`:

```bash
sudo nano /etc/fstab
```

2.  Add an entry:

```text
/dev/sdb1  /mnt/usb  ext4  defaults  0  2
```

3.  Test without reboot:

```bash
sudo mount -a
```

* * *

## 4\. Practical Examples

1.  Mount a USB drive:

```bash
sudo mount /dev/sdb1 /mnt/usb
```

2.  Mount read-only:

```bash
sudo mount -o ro /dev/sdb1 /mnt/usb
```

3.  Mount an NFS share:

```bash
sudo mount -t nfs 10.0.0.50:/data /mnt/nfs
```

4.  Mount a Windows share:

```bash
sudo mount -t cifs //10.0.0.50/shared /mnt/windows -o username=admin,password=1234
```

5.  Unmount safely:

```bash
sudo umount /mnt/usb
```

* * *

## 5\. Key Points

- **Mounting** attaches a device to the filesystem tree.
    
- **Unmounting** detaches it safely.
    
- Always unmount removable devices before removal.
    
- Use `df -h` or `mount` to verify mounted devices.
    
- Persistent mounts require `/etc/fstab`.
    

* * *

* * *

&nbsp;
