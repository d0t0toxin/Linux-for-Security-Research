* * *

**Archives:** `tar`, `cpio`, `pax`, `ar`

* * *

* * *

# Linux `tar` Command

The `tar` command in Linux is used for **archiving files**. It can combine multiple files into a single archive file, and it also supports **compression**.

* * *

## Basic Syntax

```bash
tar [options] [archive-file] [file(s)-to-archive]
```

- **archive-file**: The name of the archive to create.
    
- **file(s)-to-archive**: One or more files or directories to include.
    

* * *

## Common Options

| Option | Description |
| --- | --- |
| `c` | Create a new archive |
| `x` | Extract files from an archive |
| `t` | List contents of an archive |
| `v` | Verbose output (show files being processed) |
| `f` | Specify the archive filename |
| `z` | Compress the archive with gzip |
| `j` | Compress the archive with bzip2 |
| `J` | Compress the archive with xz |
| `--exclude='pattern'` | Exclude files matching a pattern |

* * *

## Examples

### 1\. Create a tar archive

```bash
tar -cvf archive_name.tar file1 file2 dir1/
```

- `c`: create archive
    
- `v`: verbose
    
- `f`: specify file name
    
- This creates `archive_name.tar` containing `file1`, `file2`, and `dir1/`
    

### 2\. Create a gzip compressed tar archive

```bash
tar -czvf archive_name.tar.gz file1 dir1/
```

- `z`: gzip compression
    
- Creates `archive_name.tar.gz`
    

### 3\. Create a bzip2 compressed tar archive

```bash
tar -cjvf archive_name.tar.bz2 file1 dir1/
```

- `j`: bzip2 compression

### 4\. Extract a tar archive

```bash
tar -xvf archive_name.tar
```

- `x`: extract files
    
- Extracts contents of `archive_name.tar` in current directory
    

### 5\. Extract a gzip compressed tar archive

```bash
tar -xzvf archive_name.tar.gz
```

- `z`: gzip decompression

### 6\. Extract a bzip2 compressed tar archive

```bash
tar -xjvf archive_name.tar.bz2
```

- `j`: bzip2 decompression

### 7\. List contents of a tar archive

```bash
tar -tvf archive_name.tar
```

- `t`: list files without extracting
    
- `v`: verbose, shows permissions, size, date, etc.
    

### 8\. Exclude files while creating archive

```bash
tar -cvf archive_name.tar --exclude='*.log' dir1/
```

- Excludes all `.log` files from `dir1/`

### 9\. Extract specific files from an archive

```bash
tar -xvf archive_name.tar file1.txt dir1/file2.txt
```

- Only extracts `file1.txt` and `dir1/file2.txt`

### 10\. Create an xz compressed tar archive

```bash
tar -cJvf archive_name.tar.xz dir1/
```

- `J`: xz compression

* * *

## Additional Tips

1.  **Preserve permissions and ownership**

```bash
tar -cvpf archive_name.tar dir1/
```

- `p`: preserve file permissions

2.  **Append files to an existing archive**

```bash
tar -rvf archive_name.tar new_file.txt
```

- `r`: append files to existing archive

3.  **Extract archive to a specific directory**

```bash
tar -xvf archive_name.tar -C /path/to/destination/
```

- `-C`: specify extraction directory

4.  **Create incremental backups**

```bash
tar --create --file=backup.tar --listed-incremental=snapshot.file dir1/
```

- Supports incremental backups using snapshot files

* * *

This cheat sheet covers almost everything about `tar` for day-to-day use, including creating, compressing, extracting, and listing archives.

* * *

* * *

# Linux `cpio` Command

The `cpio` command in Linux is used for **archiving files** and **copying files to/from archives**. It is an alternative to `tar` and is often used in conjunction with `find`.

* * *

## Basic Syntax

```bash
cpio [options] < [file_list]
```

or

```bash
find . | cpio [options] > archive.cpio
```

- `cpio` usually reads a **list of files from standard input** and writes an archive to standard output or a file.

* * *

## Modes of Operation

1.  **Copy-out mode (`-o`)**: Create an archive from a list of files
    
2.  **Copy-in mode (`-i`)**: Extract files from an archive
    
3.  **Pass-through mode (`-p`)**: Copy files from one directory tree to another
    

* * *

## Common Options

| Option | Description |
| --- | --- |
| `-o` | Copy-out mode (create archive) |
| `-i` | Copy-in mode (extract archive) |
| `-p` | Pass-through mode (copy directory trees) |
| `-v` | Verbose output |
| `-c` | Use ASCII (portable) headers |
| `-d` | Create directories as needed when extracting |
| `-u` | Replace files without prompting |
| `-m` | Preserve modification times |
| `--no-absolute-filenames` | Prevent absolute paths |

* * *

## Examples

### 1\. Create a `cpio` archive

```bash
find . -name '*.txt' | cpio -o > archive.cpio
```

- Finds all `.txt` files in the current directory and subdirectories
    
- `-o`: copy-out mode
    
- Redirects output to `archive.cpio`
    

### 2\. Extract a `cpio` archive

```bash
cpio -idv < archive.cpio
```

- `-i`: copy-in mode
    
- `-d`: create directories as needed
    
- `-v`: verbose
    
- Reads archive from standard input
    

### 3\. Extract archive with absolute paths removed

```bash
cpio -idv --no-absolute-filenames < archive.cpio
```

- Prevents overwriting files outside current directory

### 4\. Copy files from one directory to another (pass-through)

```bash
find /source/dir -depth | cpio -pvd /destination/dir
```

- `-p`: pass-through mode
    
- `-v`: verbose
    
- `-d`: create directories as needed
    
- Copies entire directory tree
    

### 5\. Preserve modification times when extracting

```bash
cpio -idmv < archive.cpio
```

- `-m`: preserves modification times of files

### 6\. Creating an ASCII portable archive

```bash
find . -name '*.c' | cpio -ocv > source_files.cpio
```

- `-c`: creates ASCII header for portability
    
- Useful for moving archives between different Unix systems
    

* * *

## Additional Tips

1.  **Listing contents of a `cpio` archive**

```bash
cpio -itv < archive.cpio
```

- `-t`: list contents
    
- `-v`: verbose
    

2.  **Appending to an existing archive**

```bash
find new_files/ | cpio -oA -F archive.cpio
```

- `-A`: append to archive
    
- `-F archive.cpio`: specify archive file
    

3.  **Using gzip compression with `cpio`**

```bash
find . -name '*.log' | cpio -o | gzip > logs.cpio.gz
```

- Compress archive with gzip
    
- Extract with: `gunzip -c logs.cpio.gz | cpio -idv`
    

4.  **Avoid absolute paths**

```bash
find /home/user/project -type f | sed 's|^/home/user/||' | cpio -o > project.cpio
```

- Strips leading directories before creating archive

* * *

`cpio` is a flexible command that excels when combined with `find` for selective archiving and pass-through directory copying. It's less intuitive than `tar` at first, but very powerful for scripting and backups.

* * *

* * *

# Linux `pax` Command

The `pax` command in Linux is a **portable archive interchange utility** that can read and write both `tar` and `cpio` formats. It combines features of `tar` and `cpio` and is useful for creating, listing, and extracting archives.

* * *

## Basic Syntax

```bash
pax [options] [-f archive-file] [file(s)]
```

- `-f archive-file`: specify the archive file
    
- If no archive is specified, `pax` reads from or writes to standard input/output
    

* * *

## Common Options

| Option | Description |
| --- | --- |
| `-r` | Read files from an archive (extract) |
| `-w` | Write files to an archive (create) |
| `-v` | Verbose output (show files being processed) |
| `-f file` | Specify archive filename |
| `-x format` | Specify archive format (e.g., `ustar`, `cpio`, `pax`) |
| `-s repl` | String substitution for file names during processing |
| `-p` | Preserve file permissions, ownership, and timestamps |
| `-t` | List contents of an archive |
| `-z` | Use gzip compression |
| `-k` | Keep existing files (do not overwrite) |

* * *

## Examples

### 1\. Create a `pax` archive

```bash
pax -w -f archive.pax file1 file2 dir1/
```

- `-w`: write mode
    
- `-f archive.pax`: output archive file
    
- Includes `file1`, `file2`, and `dir1/`
    

### 2\. List contents of a `pax` archive

```bash
pax -f archive.pax -v
```

- `-v`: verbose listing

### 3\. Extract files from a `pax` archive

```bash
pax -r -f archive.pax
```

- `-r`: read/extract files
    
- `-f archive.pax`: input archive
    

### 4\. Use gzip compression

```bash
pax -w -f - file1 dir1/ | gzip > archive.pax.gz
```

- Writes archive to stdout (`-f -`) and compresses with `gzip`
    
- Extract with: `gunzip -c archive.pax.gz | pax -r -f -`
    

### 5\. Extract specific files

```bash
pax -r -f archive.pax file1 file2
```

- Only extracts `file1` and `file2`

### 6\. Use string substitution

```bash
pax -w -s ',old,new,' -f archive.pax dir1/
```

- Renames files by replacing `old` with `new` in file names

### 7\. Preserve file permissions and timestamps

```bash
pax -rw -p e -f archive.pax dir1/
```

- `-rw`: read/write
    
- `-p e`: preserve permissions, ownership, and timestamps
    

### 8\. List files in a `tar` archive using `pax`

```bash
pax -f archive.tar -v
```

- Can read `tar` or `cpio` formats

* * *

## Additional Tips

1.  **Reading and writing to standard streams**

```bash
find . | pax -w -f - | gzip > archive.pax.gz
```

- Useful in pipelines for backups or compression

2.  **Convert archive formats**

```bash
pax -r -f archive.cpio -w -x ustar -f archive.tar
```

- Reads a `cpio` archive and writes a `tar` archive

3.  **Useful for scripts**

- `pax` is standardized by POSIX and works consistently across Unix-like systems

* * *

`pax` is versatile, supporting multiple archive formats and acting as a bridge between `tar` and `cpio`. It's especially useful for portable scripts and cross-platform archiving.

* * *

* * *

# Linux `ar` Command

The `ar` command in Linux is used to **create, modify, and extract from static library archives**. It is commonly used in software development for managing `.a` files (static libraries).

* * *

## Basic Syntax

```bash
ar [options] archive-file file(s)
```

- **archive-file**: Name of the archive (usually `.a` for static libraries)
    
- **file(s)**: One or more object files to include
    

* * *

## Common Options

| Option | Description |
| --- | --- |
| `r` | Insert files into the archive (replace if they already exist) |
| `c` | Create the archive if it does not exist |
| `t` | List the contents of the archive |
| `x` | Extract files from the archive |
| `d` | Delete files from the archive |
| `A` | Quickly append all members without checking timestamps |
| `v` | Verbose output |
| `s` | Create an index (symbol table) for faster linking |

> Note: `ar` does not compress files. It only bundles them.

* * *

## Examples

### 1\. Create a new archive

```bash
ar rcs libexample.a file1.o file2.o
```

- `r`: insert files (replace if they exist)
    
- `c`: create archive if it does not exist
    
- `s`: create symbol table for linking
    
- Result: `libexample.a` contains `file1.o` and `file2.o`
    

### 2\. List contents of an archive

```bash
ar t libexample.a
```

- Shows files inside `libexample.a`

### 3\. Extract files from an archive

```bash
ar x libexample.a
```

- Extracts all files to the current directory

### 4\. Extract a specific file

```bash
ar x libexample.a file1.o
```

- Only `file1.o` is extracted

### 5\. Delete a file from an archive

```bash
ar d libexample.a file2.o
```

- Removes `file2.o` from `libexample.a`

### 6\. Append files to an existing archive

```bash
ar r libexample.a file3.o
```

- Adds `file3.o` without removing existing files

### 7\. Verbose operations

```bash
ar rv libexample.a file4.o
```

- Shows files being added to the archive

### 8\. Creating a static library for linking

```bash
gcc -c file1.c file2.c
ar rcs libmylib.a file1.o file2.o
```

- Compile `.c` files to `.o` object files
    
- Bundle them into `libmylib.a` for static linking in other programs
    

* * *

## Additional Tips

1.  **Indexing is important for linking**

```bash
ar s libexample.a
```

- Ensures linker can quickly find symbols

2.  **Combine with `ranlib` (if needed)**

```bash
ranlib libexample.a
```

- Updates the symbol table on older systems

3.  **Check archive without extracting**

```bash
ar t libexample.a
```

- Quick way to see what object files are inside

4.  **Static libraries vs dynamic libraries**

- `ar` is used for **static libraries** (`.a`) only.
    
- Dynamic/shared libraries (`.so`) are created with `gcc -shared`.
    

* * *

`ar` is an essential tool in Linux development for creating and managing static libraries, especially when compiling and linking C/C++ programs.

* * *

* * *

&nbsp;
