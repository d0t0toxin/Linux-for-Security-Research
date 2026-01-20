* * *

Directory Navigation: `pwd` `cd`
Listing Contents: `ls`
File Operations: `cp` `mv` `rm` `file` `stat`
Directory Operations: `mkdir` `rmdir`
Links: `ln`


* * *

* * *

* * *

`ls` command ask the kernel for directory entries and display them. It does not read file contents. It reads metadata(names, permissions, timestamps, size, type).

`ls` works internally by the following: Shell expands wildcards (`*`) ---> `ls` receives file paths ---> Uses `stat()` system calls ---> Reads `inode` metadata ---> Formats output for terminal width.

`ls` is I/O-heavy, not CPU-heavy.

**1\. To see the file type: `>> ls -F`**

```
bin/   script.sh*   link@   data.txt   pipe|   server=
```

Explanation:

- `bin/` → directory
    
- `script.sh*` → executable file
    
- `link@` → symbolic link
    
- `pipe|` → named pipe
    
- `server=` → socket
    
- `data.txt` → regular file
    

**2\. To see all the hidden files in long listing format: `>> ls -al`**

```
-rw-r--r-- 1 user group  4096 Jan  5 14:30 file.txt
drwxr-xr-x 2 user group  4096 Jan  4 09:10 mydir
```
## 1. First character = file type
- `-` → regular file
- `d` → directory
- `l` → symbolic link
- `c` → character device
- `b` → block device
- `s` → socket
- `p` → pipe (FIFO)

## 2. Next 9 characters = permissions
Split into three groups:

- **Owner** → `rw-` (read, write)  
- **Group** → `r--` (read only)  
- **Others** → `r--` (read only)  

### Permission letters:
- `r` → read  
- `w` → write  
- `x` → execute  
- `-` → permission not granted

## 3. Number of hard links
- Shows how many hard links point to this file.  
- For directories, this usually equals: `2 + number of subdirectories`
  
## 4. Owner (user)
- The user who owns the file.

## 5. Group
- The group that owns the file.

```
lrwxrwxrwx 1 user group 11 Jan 5 12:00 link -> target
```

6. **File size**
   - Size in bytes.
   - For directories, this is the size of the directory structure, not its contents.

7. **Date & time**
   - **Last modification time (`mtime`)**
   - **Format:**
     - Month
     - Day
     - Time (or year if older than ~6 months)

8. **File or directory name**
   - The name of the file or directory.
   - Hidden files start with `.` (shown because of `-a`).

9. **Special cases you might see**
   - **Symbolic link**
     - `l` → symbolic link
     - `link` → link name
     - `target` → file it points to

10. **Special permissions**
    - You may see:
      - `s` → setuid / setgid
      - `t` → sticky bit (common in `/tmp`)


**3\. To see the allocated size of each file(number of disk blocks used) `>> ls -s` this is not same as the file size in bytes.**

```
>> ls -s 
total 4
4  mydir   0  file.txt
```

`total` shows the total disk blocks used, typically shown in 1K blocks (depends on system).

To see directory content size use: `>> du -s mydir`

**4\. Linux files have three different times, and `ls -lc` `ls -lu` lets you choose which one to display or sort by.**

```
>> ls -l --full-time
-rw-r--r-- 1 user user 1234 2026-01-05 14:30:22.000000000 file.txt
```

**mtime** → Modification time (file contents changed): default  
**ctime** → Change time (metadata changed): `-c`  
**atime** → Access time (file read): `-u`

\*\*Sorting by time:  
\*\*a. Sort by modification time (default): `>> ls -lt`  
b. Sort by change time: `>> ls -ltc`  
c. Sort by access time: `>> ls -ltu`  
d. Reverse order (oldest first): `>> ls -ltr`

**5\. `ls` vs `stat`**

`ls` shows directory contents, gives a summary view of files and it is best for quick browsing.  
`stat` gives detailed metadata of a single file, best for inspection and debugging.

```
>> stat file
  File: file
  Size: 25              Blocks: 8          IO Block: 4096   regular file
Device: 259,2   Inode: 29491349    Links: 1
Access: (0664/-rw-rw-r--)  Uid: ( 1000/    d0t0)   Gid: ( 1000/    d0t0)
Access: 2025-12-29 09:20:07.598292973 +0545
Modify: 2025-12-29 09:19:55.794091260 +0545
Change: 2025-12-29 09:19:55.794091260 +0545
 Birth: 2025-12-29 09:19:55.794091260 +0545
```

* * *

* * *

* * *

`cp` stands for copy, it purpose is to Create a new file or directory that is a duplicate of another.

`cp` copies file contents and some or all metadata(permissions, timestamps, ownership — depending on options).

`cp` Works Internally by the following: Opens source file (read-only) → Creates destination file → Reads data in blocks → Writes data to destination → Copies metadata (optional) → Closes file descriptors

**It uses system calls like:**

- `open()`
- `read()`
- `write()`
- `stat()`
- `fchmod()`, `fchown()`

**1\. Destination is silently overwritten by default using `cp` so we can use `-i` combined with `-v` for verbose for interactive, we can specify yes or no for overwrite.**

```
>> cp -vi file.txt out.txt 
cp: overwrite 'out'? yes
'file.txt' -> 'out.txt'
```

**2\. Using option `-n` to check if the file exist than drop the copy and if the file does not exist than a new file is created with the copied content of the source file.**

```
>> cp -n file1.txt output.txt
```

**3\. `cp` does not copy directories, so we have to specify `-r` or sometimes `-R` flag for recursive.**

```
>> cp -r dir1 dir2 

```

**4\. While coping, by default the permissions, ownership may change and timestamps may reset so we can use `-a` for archive to Preserving Metadata.**

```
>> cp -a source dest
```

**5\. When we copy files with `cp`, symbolic links (symlinks) can be handled in different ways.**

```
>> echo "Hello" > real.txt
>> ln -s real.txt link.txt
>> ls 
real.txt        (regular file)
link.txt  -> real.txt   (symlink)
```

**`cp` dereference by default which simply means it copies the content of the target, equivalent to `-L` option, symlink is not preserved.**

```
>> cp link.txt copy.txt   #(regular file, contains "Hello")
```

**`-d` and `-P` can be used to preserve symlinks, copies the link itself.**

```
>> cp -P link.txt copy.txt # copy.txt -> real.txt
```

**5\. Hard Links vs Copies**

Hard Links are multiple directory entries pointing to the same inode(multiple files with same inode).

```
file1.txt ─┐
file2.txt ─┴──▶ inode #12345
```

If we delete one name `rm file1.txt`, the inode and data remains until the `link count = 0`

But symlinks have there own inode.

```
lrwxrwxrwx 1 user user 28 Jan 6 10:00 link -> taget

link = inode #20000
target = inode #12345
```

**What is Inode?** --> An inode (index node) is a data structure that stores metadata about a file, not the filename. Think of it like an `ID card` for a file.

An Inode does not store file data directly but it store pointers to disk blocks. We can see inode number per each files: `>> ls -i file.txt` or `>> stat file.txt`, each file have a unique inode.

## Directory Entry

```
+----------------+
| file.txt       | ----> inode #12345
+----------------+
```

## inode #12345

```
+--------------------------+
| Permissions: rwx         |
| Owner: UID / GID         |
| Size: 1024 bytes         |
| Timestamps: atime, mtime, ctime |
| Data pointers ---------> file contents on disk
+--------------------------+
```

## What an inode stores

- **File type**: regular, directory, symlink, device, etc.
    
- **Permissions**: read, write, execute (rwx)
    
- **Owner and group**: UID and GID
    
- **File size**
    
- **Timestamps**:
    
    - **atime**: last access time
        
    - **mtime**: last modification time
        
    - **ctime**: inode change time
        
- **Number of hard links**
    
- **Pointers** to the file’s data blocks

**Linux Systems have the following inode timestamps we can see it with `stat` command:**  
atime → Last read  
mtime → Content modified  
ctime → Metadata Changed  
birth → Creation time (optional)

We can create a hand link copy `file2.txt`. The data are synced together.

```
>> ln file1.txt file2.txt
>> ls -li                                                 
29492786 -rw-rw-r-- 2 user group 91 Jan  6 14:40 file1.txt                     
29492786 -rw-rw-r-- 2 user group 91 Jan  6 14:40 file2.txt
```

**6\. We can copy files which are new to the destination: `>> cp -u source dest`**

Used to add file in backup directories or to sync operations.

**7\. We can create full directory tree to the destination: `>> cp --parents path/to/file /backup` `# /backup/path/to/file`**

**8\. Suppose we have `file.txt` in `dest/` already but we don't want to overwrite, what we can do is to make that existing file as backup.**

```
cp --backup=numbered file.txt dest/
```

Creates these files in `dest/` directory:

```
file.txt
file.txt.~1~

```

**#Performance Consideration:**  
**9\. Fast Copying with Copy-on-Write**  
On filesystems that support copy-on-write (such as **Btrfs** and **XFS**), `cp` can create near-instant copies using reflinks:

```
cp --reflink=auto source destination
```

When supported, the data blocks are shared instead of duplicated, saving both time and disk space. If the filesystem does not support reflinks, `cp` silently falls back to a normal copy.

**10\. Avoid Unnecessary Metadata Preservation**  
In some scenarios, preserving certain metadata can add overhead:

```
cp --no-preserve=links source destination
```

This avoids copying symbolic link metadata, which can be useful when links are irrelevant or when optimizing bulk operations.

**#Sorting Overhead**  
Unlike tools such as `ls`, the `cp` command does **not** perform any internal sorting of files. Therefore, no sorting-related performance overhead exists when copying multiple files.

**11\. `cp` and Wildcards**

`cp` itself does not understand wildcards. Pattern expansion is handled entirely by the shell:

```
cp *.txt backup/
```

> **Warning**  
> The shell expands `*.txt` into a list of matching filenames **before** `cp` is executed.  
> If no files match, behavior depends on the shell configuration and may result in an error.

**12\. Copying Special Files**  
**#What are Device Nodes?**  
In Linux and Unix-like systems, a device node (also called a special file) is a file that represents a hardware device or a virtual device.

- They live in the filesystem (usually under `/dev`)
- They act as an interface to the device, so programs can read from or write to them like normal files.
- The OS handles the actual communication with the hardware.

In other words:  
Device nodes let you treat devices like files.

**#There are two main types:**  
a) Character devices (c)

- Data is handled byte by byte.
- Example: /dev/tty (terminals), /dev/console, /dev/null
- Operations are sequential, no buffering.

```
>> ls -l /dev/null
crw-rw-rw- 1 root root 1, 3 Jan 1 00:00 /dev/null # 'c' at the start = character device
```

b) Block devices (b)

- Data is handled in blocks.
- Example: `/dev/sda` (entire hard disk), `/dev/sdb1` (partition)
- Can be randomly accessed, like files on a disk.

```
>> ls -l /dev/sda
brw-rw---- 1 root disk 8, 0 Jan 1 00:00 /dev/sda       # 'b' at the start = block device
```

Example:  
`/dev/null` → character → Discards all inputs; returns EOF on read  
`/dev/zero` → character → Products endless zeros when read  
`/dev/tty` → character → Terminal input/output  
`/dev/sda` → block → First hard disk  
`/dev/sda1` → block → First partition on first hard disk  
**13\. We can copy special files on archive mode to preserve metadatas:**

```
cp -a /dev/null backup/
```

The `-a` (archive) option preserves file type, permissions, ownership, and timestamps.  
Without `-a`, copying special files may result in broken or unusable copies.

**#Debugging `cp`**

**14.We can observe how `cp` interacts with the system at runtime, use `strace`:**

```
strace cp file dest
```

This reveals low-level system activity, including:

- `open`, `read`, and `write` system calls
- Permission and access failures
- Filesystem-related errors

`strace` is invaluable when diagnosing unexpected behavior.  
**15\. When Not to Use `cp`**  
Although `cp` is excellent for simple file copying, it is not ideal for:

- Large-scale backups
- Network synchronization
- Incremental or resumable copying

In these cases, prefer tools designed for efficiency and reliability: `rsync -a`

`rsync` minimizes data transfer, supports incremental updates, and provides robust error handling—making it better suited for complex copy operations.

* * *

* * *

* * *

`ln` command is a fundamental utility in Unix-like systems, enabling you to create links between files. Links are like pointers or references, allowing multiple names to refer to the same data.  
Symlinks (symbolic links) are special files that point to another file or directory—similar to shortcuts in Windows or aliases in macOS.

At its core, `ln` creates connections between files in the filesystem. There are two types of links:  
**a. Hard Links (Default)**  
A hard link is another name for the same file. Multiple hard links share the same inode, meaning they point to the exact same data on disk.  
**Characteristics:**

- Cannot cross filesystem boundaries.
- Cannot normally link directories (restricted to prevent cycles).
- Survives if the original file is deleted; the data remains as long as one link exists.

**b. Symbolic Links (Symlinks)**  
A symlink does not contain the actual data. It only stores the path to another file or directory (**the target**).   
`symlink ──▶ target file or directory`  
If the target moves or is deleted, the symlink breaks.  
**Characteristics:**

- Can cross filesystem boundaries.
- Can link directories.
- Breaks if the target is removed or moved.

**1\. By default, `ln` creates a hard link. The `-s` option changes this behavior and tells `ln` to create a symbolic link (symlink) instead.**

```
>> ln -s /home/user/docs/report.txt report-link.txt
>> ls -l
lrwxrwxrwx 1 user group 28 Jan 6 10:00 report-link.txt -> /home/user/docs/report.txt
```

**2\. If the TARGET is a symlink, `-L` tells `ln` to follow it and create a link to the file it points to, not the symlink itself.**

```
>> ls -li symlink.txt
22222 lrwxrwxrwx 1 user user symlink.txt -> real.txt
>> ln -L symlink.txt newlink
>> ls -li real.txt symlink.txt newlink
11111 -rw-r--r-- 2 user user real.txt
22222 lrwxrwxrwx 1 user user symlink.txt -> real.txt
11111 -rw-r--r-- 2 user user newlink
```

**3\. Link the Symlink Itself, which means `-P` tells `ln` not to follow symlinks.**

```
>> ln -P symlink.txt link_to_symlink
>> ls -li symlink.txt link_to_symlink
22222 lrwxrwxrwx 2 user user symlink.txt -> real.txt
22222 lrwxrwxrwx 2 user user link_to_symlink -> real.txt
```

Same inode → same symlink.  
**4\. Creating relative symlinks. By default, symlinks store exactly the path you give (often absolute). `-r` tells `ln` to calculate a relative path from `LINK_NAME` to `TARGET`.**

```
ln -sr /usr/local/bin/python ~/bin/python
```

Stored path might become:

```
../../usr/local/bin/python
```

**5\. Safest manual symlink:**

```
ls -siv target link
```

**6\. Safe overwrite with backups:**

```
ls -s --backup=numbered -v target link
```

**7\. Best practice for dt files:**

```
ln -srv ~/.dotfiles/.vimrc ~/.vimrc
```

**8\. Bulk linking to a directory:**

```
ln -sv file1 file2 -t ~/bin
```

**9\. Script-safe linking(used by package managers):**

```
ls -snf target link
```

* * *

* * *

* * *

`mv` stands for move, but that name is misleading. Rename files/directories OR move them to a different location.

**1\. By default, `mv` renames a file if SOURCE and DEST are in the same directory**

```shell
>> ls -li old.txt
12345 -rw-r--r-- 1 user user old.txt

>> mv old.txt new.txt

>> ls -li new.txt
12345 -rw-r--r-- 1 user user new.txt
```

What actually happens:

- Filename changes
    
- Inode stays the same
    
- No data is copied
    

**2\. If DEST is a directory, `mv` moves the file into that directory**

```shell
>> ls -li file.txt
55555 -rw-r--r-- 1 user user file.txt

>> mv file.txt docs/

>> ls -li docs/file.txt
55555 -rw-r--r-- 1 user user docs/file.txt

```

**3\. Moving across filesystems changes inode (copy + delete)**

```shell
>> ls -li big.iso
77777 -rw-r--r-- 1 user user big.iso

>> mv big.iso /mnt/usb/

>> ls -li /mnt/usb/big.iso
88888 -rw-r--r-- 1 user user big.iso

```

Internally:

1.  Copy data to `/mnt/usb/`
    
2.  Verify copy
    
3.  Delete original file
    

**4\. `mv` silently overwrites destination by default**

```shell
>> mv a.txt b.txt

```

If `b.txt` exists: It is deleted → `a.txt` takes it place → no warning

**5\. `-i` makes `mv` interactive (safe mode)**

```shell
>> mv -i a.txt b.txt
mv: overwrite 'b.txt'?

```

Prevents accidental data loss  
Most admins alias this by default

```shell
alias mv='mv -i'

```

**6\. `-f` forces overwrite (even if `-i` is set)**

```shell
>> mv -f a.txt b.txt

```

No prompt, overwrites safety.

**7\. `-n` prevents overwrite completely**

```shell
>> mv -n a.txt b.txt

```

If `b.txt` exists: `a.txt` is not moved, no error is thrown.

**8\. `-v` shows exactly what `mv` is doing**

```shell
>> mv -v file.txt docs/
renamed 'file.txt' -> 'docs/file.txt'

```

Very useful in scripting and learning.

**9\. Moving multiple files requires DEST to be a directory**

```shell
>> mv a.txt b.txt c.txt backup/

```

**10\. `mv` works on directories the same way as files**

```shell
>> ls -ldi old_dir
99999 drwxr-xr-x 2 user user old_dir

>> mv old_dir new_dir

>> ls -ldi new_dir
99999 drwxr-xr-x 2 user user new_dir


```

Directory renamed, Contents untouched, same inode for directory.

**11\. Permissions: `mv` checks directory permissions, not file permissions**

We need write permissions on both directories source and destination.

```shell
-r--r--r-- 1 user group file.txt

```

We can still move it (as long as directory is  writeable)

**12\. Hidden files are NOT matched by `*`**

```shell
>> mv * backup/
```

Does not move

- `.bashrc`
- `.gitconfig`

Correct Way:

```shell
>> mv .[^.]* * backup/
```

**13\. Moving and renaming in one command**

```shell
>> mv report.txt archive/report_2025.txt
```

**14\. `mv` is atomic only on the same filesystem**

Atomic:

```shell
>> mv temp.conf final.conf
```

Not atomic:

```shell
>> mv temp.conf /mnt/usb/final.conf
```

This is why Package Managers rely on `mv`.

* * *

* * *

* * *

The `rm` command stands for *remove*, and its sole purpose is to delete files and directories permanently.

Unlike graphical user interfaces that often move deleted files to the Trash, `rm` bypasses the trash altogether. This means:

- Deletion is immediate and permanent.
    
- Once a file is deleted with `rm`, it’s generally difficult—if not impossible—to recover.
    

**Why rm command is dangerous?**  
When we execute the command:

- Linux removes the directory entry (the file's reference in the file system).
- It decreases the file’s inode link count.
- If no other links exist to that file, the disk space is marked as free, though the data remains intact until overwritten.

Data is not immediately wiped, but the pointer to it is gone, making it much harder to recover.

To delete a file with `rm`, we need write permission on the directory containing the file, not necessarily the file itself.  
This often surprises beginners because file permissions may allow reading or modifying a file, but not deleting it.

**1\. `rm` can not delete directories by default. To remove directories and it contents we have to use `-r` flag for recursive removal:**

```
>> rm -rf dir_filestore
```

**2\. The `-f` flag forces rm to delete files without asking for confirmation. It also ignores nonexistent files and suppresses prompts.**

```bash
>> rm -f file.txt
```

**3\. The `-i` flag prompts you before every deletion, making it safer:**

```bash
>> rm -i file
```

You can even set a safer alias: `alias rm='rm -i'`

**4, Hidden files are not matched by `*` by default. To delete hidden files, we must explicitly specify them:**

```bash
>> rm -r .config
```

**5\. When we delete symbolic link with `rm`, it removes the symlink itself not the file or directory it points to.**

**6\. A hard link is another reference to the same file on disk. When you delete a hard link with rm, the data is not removed until all hard links pointing to the file are deleted.**

**7\. In scripts, it is very important to ensure that `rm` commands are safe and fail gracefully:**

```bash
>> rm -rf .config || exit 1
```

**8\. Mental model: `rm` removes references, not the actual data. Once removed, there's no forgiveness.**

* * *

* * *

* * *

The `rmdir` command is used to **remove empty directories**. It is a safer alternative to `rm -r` because it will only delete directories that contain no files or subdirectories. `rmdir` will fail if the directory is not empty.

**1\. The `-p` option allows you to remove nested empty directories. It will remove directories in a chain, provided each one is empty:**

```bash
>> rmdir -p a/b/c
```

This command removes `c`, then `b`, then `a`, **only if** they are all empty.

**2\. We do not need permission to delete the directory itself, but we need write and execute permisssions on the parent directory.**

**3\. We can use `find` in combination with `rmdir` to remove only empty directories:**

```bash
>> find . -type d -empty -exec rmdir {} +
```

This will search for empty directories and remove them one by one, avoiding errors from non-empty directories.  
More preferred way: To safely remove empty directories without risk of deleting files, use:

```bash
>> find /tmp/myapp -type d -empty -delete
```

This command finds all empty directories in /tmp/myapp and deletes them, ensuring no non-empty directories are affected.

**4, In scripts, we can suppress errors if directories are not empty:**

```bash
>> rmdir dir 2>/dev/null
```

This prevents the script from crashing if a directory is not empty.

* * *

* * *

* * *

The `cd` command means **change directory**. It is used to change the shell’s *current working directory*—the directory where commands run and files are resolved.

#### Important facts:

- `cd` is a **shell builtin**, not an external program.
    
- It **cannot** be a standalone binary like `/bin/cd`.
    
- It affects **only the current shell process**.
    
- A child process cannot change the parent shell’s directory.
    

This is why running `cd` inside a script or subshell behaves differently than in your interactive shell.

**Special Directory Symbols**  
Symbol Meaning  
`.` Current directory  
`..` Parent directory  
`~` Home directory  
`-` Previous directory

**1\. `cd` `cd ~` and `cd /home/user`, these will change the current working directory to home no matter where you are.**

**2\. Two environment variables are involved `$PWD` → Current working directory and `$OLDPWD` → Previous working directory.**

```bash
>> echo echo $OLDPWD                                             
/home/d0t0/hackthebox/Sorcery
>> echo $PWD
/home/d0t0
```

**3\. To `cd` into a directory, we need execute(`x`) permission on the directory**

**4\. If we `cd` into a symlink, the shell remembers the logical path (the symlink), not the real filesystem path.**

**5\. Logical vs Physical Mode (-L / -P)**

```bash
>> ln -s /tmp link
>> cd link
>> pwd -L
/home/d0t0/link
>> pwd -P
/tmp
```

Likewise: with `cd`

```bash
>> cd -P link #for Physical path
>> cd -L link #for logical path
```

**6\. We always check if the `cd` succeeds, when we use in a script.**

```bash
>> cd /path || exit 1
```

**7\. If we use `cd` in a webshell the directory change only happens inside the webshell and the parent shell remoins unchanged.**

```
(cd /path && touch my.file)
```

**8\. Safe directory change while scripting commanded:**

```bash
>> cd /path || { echo "failed"; exit 1; }
```

* * *

* * *

* * *

`pwd` stands for: Print Working Directory. It prints the absolute path of the directory your shell is currently inside.  
`pwd` is shell builtin.  
**1\. There are two Environment Variables involved `$PWD` → shows logical current directory`pwd -L` and `$OLDPWD` → Previous directory used by `cd -`**

**2\. The best practice to use `pwd` in shell scripting is like this.**

```bash
DIR="$(pwd -P)"
```

**3\. Debugging tips: Check what shell believes the Current Working Directory is:**

```bash
>> declare -p PWD 
export PWD=/home/d0t0/test_for_admin
```

* * *

* * *

* * *

`mkdir` (**make directory**) is a core Unix/Linux command used to **create directories** in the filesystem.  
In scripting, `mkdir` is not just for creating folders—it is used for:

- Preparing execution environments
    
- Ensuring idempotent scripts
    
- Managing permissions securely
    
- Building dynamic directory trees
    
- Handling errors safely in automation
    

**1\. Parent Dierctory Creation**

```bash
>> mkdir -p a/b/c
```

Key Properties of `-p`

- Creates missing parent directories
- Does not fail if directory already exists
- Essential for idempotent scripts

Script-safe pattern:

```bash
>> mkdir -p /var/myapp/{data,logs,cache}
```

**2\. We can set permissions at creation with `-m` flag.**

```
>> mkdir -m 755 secure_dir
```

**3\. We can create dynamic directory names using variables**

```bash
>> DATE=$(date +%y--%m--%d)
>> echo $DATE
26--01--15
>> mkdir -p /backups/$DATE
```

**4\. Creating dynamic directory names using loops.**

```bash
for user in Ram Sita Hari; do 
    mkdir -p /home/$user/{docs, downlods, scripts}
done
```

**5\. This is how it is used in professional scripting. Example: Some Application Deployment.**

```bash
APP_ROOT=/opt/myapp/
mkdir -p \
    $APP_ROOT/{bin, conf, logs, data, tmp}
mkdir -m 750 $APP_ROOT/logs
```

* * *

* * *

* * *

The `file` command determines the type of a file by examining its content, not its filename or extension. Unlike `ls` or `stat`, `file` reads *magic numbers* and internal structure.

**How `file` Works Internally:**  
The `file` command relies on Magic Database located at `/usr/share/misc/magic` which contains patterns used to identify file types.

**Detection Order**

- Filesystem checks (empty, special)
- Magic numbers
- Character encoding
- Fallback heuristics

**Important Options (Advanced Focus)**  
Option Meaning  
`-b` Brief output (no filename)  
`-i` MIME type  
`-z` Examine compressed files  
`-L` Follow symlinks  
`-s` Read block special files  
`--mime-encoding` Encoding only

**1\. `File` in Automation and Script.**

- `$1` → First command-line argument
- `$@` → All command-line arguments (as separate words)
- \*\*\*\*\*\*`$#` → Total number of command-line arguments

A. **Conditional Logic**: It checks whether the file passed as the first argument is a text file and prints a message if it is.  
**script.sh**

```bash
#!/usr/bin/env bash
if file "$1" |grep -q "text"; then
 echo "Text file detected"
fi
```

```
>> chmod +x script.sh
>> ./script.sh file.txt
Text file detected
```

MIME (Multipurpose Internet Mail Extensions) in the web is a standard that tells browsers what type of content is being sent by a server. MIME defines the format of a file or data so the browser knows how to handle it (display it, download it, play it, etc.).  
**Example:**  
When a server sends a file, it includes a MIME type like:

- `text/html` → HTML web page
- `text/css` → CSS file
- `application/javascript` → JavaScript file
- `image/png` → PNG image
- `application/json` → JSON data

B. **MIME-Based Decisions which is the best practice.**

```bash
#!/usr/bin/env bash
MIME=$(file -b --mime-type "$1")
case "$MIME" in 
 text/*) echo "Text File!";;
 image/*) echo "Image File!";;
 application/pdf) echo "PDF File!";;
esac
```

**2\. Some Advanced Use Cases:**

a. Detecting the character encoding of a text file (not the language, but how bytes are interpreted as characters).

```bash
>> file --mime-encoding file.txt
file.txt: us-ascii
```

b. Process Compressed Files,`-z` tells `file` to look inside compressed files. It analyzes the uncompressed content, not just the archive type.

*Supported compression formats*

- gzip (`.gz`)
    
- bzip2 (`.bz2`)
    
- xz (`.xz`)
    
- compress (`.Z`)
    

```
file -z archive.gz
```

It help to inspect safely without extracting.

c. Analyzing input streams:

```bash
>> echo -n "This is the information of the system in the formatx"|base64 > encoded.txt
>> cat encoded.txt 
VGhpcyBpcyB0aGUgaW5mb3JtYXRpb24gb2YgdGhlIHN5c3RlbSBpbiB0aGUgZm9ybWF0eA==
>> base64 -d encoded.txt| file - 
/dev/stdin: ASCII text, with no line terminators
```

Advanced shell scripting is **stream-based**, not file-based.

This enables:

- Inspecting downloads
    
- Checking decrypted data
    
- Validating transformed streams
    

**3\. Integrating `file` commands with other tools.**  
a. We can recursively finds all regular files in the current directory and prints their file types efficiently.

```
>> find . -type f -exec file {} +
./encoded.txt:   ASCII text
./file.txt:      ASCII text
./something.pdf: empty
./script.sh:     Bourne-Again shell script, ASCII text executable
./magic.mgc:     magic binary file for file(1) cmd (version 20) (little endian)
```

b. We can recursively lists all regular files in the current directory and prints their file types.

```
>> find . -type f|xargs file
./encoded.txt:   ASCII text
./file.txt:      ASCII text
./something.pdf: empty
./script.sh:     Bourne-Again shell script, ASCII text executable
./magic.mgc:     magic binary file for file(1) cmd (version 20) (little endian)
```

c. We can lists only the filenames in the current directory whose contents are detected as text files.

```bash
>> file * |awk -F: '/text/{print $1}' 
encoded.txt
file.txt
script.sh
```

* * *

* * *

* * *

The `stat` command displays detailed metadata about files and directories as stored by the filesystem.

Unlike `ls`, which is for humans, `stat` is for scripts and system logic.

```bash
>> stat file.txt 
  File: file.txt
  Size: 41              Blocks: 8          IO Block: 4096   regular file
Device: 259,2   Inode: 28966978    Links: 1
Access: (0664/-rw-rw-r--)  Uid: ( 1000/    d0t0)   Gid: ( 1000/    d0t0)
Access: 2026-01-15 20:24:23.420602654 +0545
Modify: 2026-01-15 20:24:14.531748702 +0545
Change: 2026-01-15 20:24:14.536431138 +0545
 Birth: 2026-01-15 20:24:14.531748702 +0545
```

***It is very critical to Understanding File Timestamps:***

stat shows three core timestamps:

Time Meaning  
Access (atime) Last read  
Modify (mtime) Last content change  
Change (ctime) Last metadata change

**1\. We have some very basic and important options:**  
`-c` Custom format (MOST IMPORTANT)  
`-f` Filesystem info  
`-L` Follow symlinks  
`-t` Terse output

**2\. We have custom formatting with `stat -c`**  
Common Format Specifiers  
Specifier Meaning  
`%n` File name  
`%s` File size (bytes)  
`%A` Permissions (rwx)  
`%a` Permissions (octal)  
`%U` Owner  
`%G` Group  
`%i` Inode  
`%y` Modification time  
`%F` File type

* * *

* * *

* * *

The `touch` command:

1.  Creates empty files if they do not exist
2.  Updates timestamps if they do  
    `touch` does not modify file contents—only metadata.

**Linux tracks three timestamps:**  
Timestamp Meaning  
`atime` Last access  
`mtime` Last content modification  
`ctime` Last metadata change

**`touch` can modify:**

- atime
- mtime
- It cannot directly modify ctime

**Option Purpose**  
`-a` Change access time only  
`-m` Change modification time only  
`-t` Set custom timestamp  
`-d` Human-readable date  
`-r` Copy timestamp from another file  
`-c` Do not create file

**1\. We can create batch files at once.**

```bash
>> touch file{1..10}.txt
```

**2\. We update only log files like this:**

```bash
>> find . -type f -exec touch {} +
```

**3\. Finds all regular files older than 7 days in the current directory tree and updates their timestamps to the current time.**

```bash
>> find . -type f -mtime +7 -exec touch {} +
```

**4\. Real world professional scripts: Auto-refresh cache files.**

```bash
>> find cache/ -type f -mtime +1 -exec touch {} +
```

* * *

* * *

* * *

&nbsp;
