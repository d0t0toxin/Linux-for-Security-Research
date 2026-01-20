* * *

**Disk Space Usage:** `df`, `du`

* * *

* * *

# Linux `df` Command

The `df` command in Linux is used to **display disk space usage** for mounted filesystems. It provides details like total space, used space, available space, and mount points.

* * *

## Basic Syntax

```bash
df [options] [filesystem]
```

- `[options]`: Modify command output
    
- `[filesystem]`: Check a specific filesystem or directory
    

* * *

## Understanding Output

Typical `df` output:

```bash
Filesystem     1K-blocks    Used Available Use% Mounted on
/dev/sda1      50000000 25000000  25000000  50% /
```

| Column | Meaning |
| --- | --- |
| Filesystem | Device name or filesystem |
| 1K-blocks | Total size in 1 KB blocks |
| Used | Space used |
| Available | Space available for users |
| Use% | Percentage of space used |
| Mounted on | Mount point |

* * *

## Common Options

| Option | Description |
| --- | --- |
| `-h` | Human-readable sizes (e.g., MB, GB) |
| `-H` | Human-readable, powers of 1000 (e.g., 1GB = 1000MB) |
| `-T` | Show filesystem type |
| `-a` | Show all filesystems, including dummy and zero-size |
| `-i` | Show inode usage instead of block usage |
| `--total` | Display a total line with combined sizes |

* * *

## Practical Examples

### 1\. Basic Usage

```bash
df
```

Displays disk usage for all mounted filesystems.

### 2\. Human-Readable Format

```bash
df -h
```

Output example:

```
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        50G   25G   25G  50% /
```

### 3\. Show Filesystem Types

```bash
df -T
```

Output example:

```
Filesystem     Type  1K-blocks    Used Available Use% Mounted on
/dev/sda1      ext4  50000000 25000000 25000000  50% /
```

### 4\. Check Specific Filesystem or Directory

```bash
df -h /home
```

Shows usage for `/home` filesystem.

### 5\. Show All Filesystems Including Dummy

```bash
df -a
```

Includes virtual or zero-size filesystems like `tmpfs`.

### 6\. Check Inode Usage

```bash
df -i
```

Shows inodes instead of disk space, useful when `disk full` error occurs due to too many small files.

### 7\. Display Total Usage Across Filesystems

```bash
df -h --total
```

Adds a total line at the bottom with combined disk usage.

* * *

## Summary

- `df` is essential for monitoring disk usage.
    
- Use `-h` for human-readable format.
    
- Use `-T` to see filesystem type.
    
- Use `-i` to check inode usage.
    
- Combine options for detailed information.
    

* * *

* * *

# Linux `du` Command Guide

The `du` command in Linux is used to **estimate the disk space used by files and directories**. It is useful for identifying which files or directories are consuming the most space.[](https://chatgpt.com/c/696f6a3a-3d08-8327-b65a-aa26576d30e5#differences-between-du-and-df)

* * *

## Basic Syntax

```bash
du [options] [directory_or_file]
```

- `[options]` → optional flags to modify output
    
- `[directory_or_file]` → target directory or file (default is current directory)
    

* * *

## Understanding Output

Example:

```bash
du /home/user
```

```
4	/home/user/Documents
12	/home/user/Downloads
16	/home/user
```

- Numbers are in **blocks** (usually 1 KB by default)
    
- Last line shows the **total space used** by the directory
    

* * *

## Common Options

| Option | Description |
| --- | --- |
| `-h` | Human-readable sizes (KB, MB, GB) |
| `-s` | Summarize; show total size only |
| `-a` | Show disk usage for all files, not just directories |
| `--max-depth=N` | Limit output to N levels of subdirectories |
| `-c` | Produce a grand total at the end |
| `-x` | Skip directories on different filesystems |
| `-L` | Follow symbolic links |

* * *

## Practical Examples

### 1\. Basic Usage

```bash
du
```

Shows disk usage for each directory in the current folder.

### 2\. Human-Readable Format

```bash
du -h
```

Output example:

```
4.0K    ./Documents
12K     ./Downloads
16K     .
```

### 3\. Summarize Total Size

```bash
du -sh /home/user
```

Output:

```
16K /home/user
```

### 4\. Show All Files

```bash
du -ah /home/user
```

Includes **both files and directories**.

### 5\. Limit Depth

```bash
du -h --max-depth=1 /home/user
```

- Shows sizes of `/home/user` and its **immediate subdirectories**.

### 6\. Show Grand Total

```bash
du -ch /home/user
```

- `-c` adds a **total line** at the end.

### 7\. Check Disk Usage for a Single File

```bash
du -h /home/user/file.txt
```

### 8\. Skip Other Filesystems

```bash
du -xh /mnt
```

- Skips directories that are mounted from **other filesystems**.

* * *

## Differences Between `du` and `df`

| Command | Purpose |
| --- | --- |
| `df` | Disk usage of **entire filesystems** |
| `du` | Disk usage of **files and directories** |

* * *

## Summary

- `du` estimates disk space usage at **directory and file level**.
    
- Use `-h` for human-readable output.
    
- Use `-s` to summarize total size.
    
- Use `--max-depth` to limit directory levels.
    
- Ideal for identifying **which directories or files consume most disk space**.
    

* * *

* * *

&nbsp;
