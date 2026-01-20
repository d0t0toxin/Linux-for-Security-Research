* * *

**Compression / Decompression:** `gzip`, `gunzip`, `bzip2`, `bunzip2`, `xz`, `unxz`, `zip`, `unzip`, `zstd`

* * *

* * *

* * *

# `Gzip` Command in Linux

`gzip` is a widely used command-line utility in Linux for file compression. It compresses files to reduce their size and uses the `.gz` extension for compressed files.

* * *

## Basic Syntax

```bash
gzip [options] filename
```

- `filename` – The file you want to compress.

* * *

## Common Options

| Option | Description |
| --- | --- |
| `-d` | Decompress the file (same as `gunzip`) |
| `-c` | Write output to standard output (stdout) and keep original file |
| `-k` | Keep the original file |
| `-r` | Recursively compress files in directories |
| `-l` | List compressed file contents |
| `-t` | Test compressed file integrity |
| `-v` | Verbose mode, shows compression ratio |
| `-1` to `-9` | Compression level (`-1` fastest, `-9` best compression) |

* * *

## Examples

### 1\. Compress a Single File

```bash
gzip file.txt
```

- Output: `file.txt.gz`
    
- Original `file.txt` is replaced by compressed file.
    

### 2\. Keep Original File

```bash
gzip -k file.txt
```

- Output: `file.txt.gz`
    
- Original `file.txt` remains.
    

### 3\. Compress Multiple Files

```bash
gzip file1.txt file2.txt file3.txt
```

- Each file is compressed individually:
    
    - `file1.txt.gz`, `file2.txt.gz`, `file3.txt.gz`

### 4\. Compress Recursively

```bash
gzip -r my_folder/
```

- Compresses all files in `my_folder` and its subdirectories.

### 5\. Check Compression Ratio Verbosely

```bash
gzip -v file.txt
```

- Example output:

```
file.txt: 75.2% -- replaced with file.txt.gz
```

### 6\. Compress to Standard Output

```bash
gzip -c file.txt > file.txt.gz
```

- `-c` sends compressed data to stdout, allowing redirection.

### 7\. Test Compressed File Integrity

```bash
gzip -t file.txt.gz
```

- Returns nothing if the file is okay.

### 8\. List Contents of Compressed File

```bash
gzip -l file.txt.gz
```

- Example output:

```
         compressed   uncompressed  ratio uncompressed_name
               78              300  74.0% file.txt
```

### 9\. Decompress File

```bash
gzip -d file.txt.gz
```

- Or using `gunzip`:

```bash
gunzip file.txt.gz
```

- Restores the original `file.txt`.

### 10\. Use Different Compression Levels

```bash
gzip -9 file.txt   # Best compression
gzip -1 file.txt   # Fastest compression
```

* * *

## Notes

- `gzip` works on single files; to compress directories as a single file, use `tar` with `gzip`:

```bash
tar -czvf archive.tar.gz folder_name/
```

- Decompress using:

```bash
tar -xzvf archive.tar.gz
```

* * *

`gzip` is efficient, simple, and widely supported, making it essential for file compression in Linux.

* * *

* * *

# `Gunzip` Command in Linux

`gunzip` is the command-line utility in Linux used to decompress files that were compressed using `gzip`. It restores `.gz` files to their original form.

* * *

## Basic Syntax

```bash
gunzip [options] filename.gz
```

- `filename.gz` – The compressed file you want to decompress.

> Note: `gunzip` is equivalent to `gzip -d`.

* * *

## Common Options

| Option | Description |
| --- | --- |
| `-c` | Write output to stdout, keep original file |
| `-f` | Force decompression even if file exists |
| `-k` | Keep the original compressed file |
| `-l` | List compressed file information |
| `-r` | Recursively decompress files in directories |
| `-t` | Test compressed file integrity |
| `-v` | Verbose mode, shows decompression process |

* * *

## Examples

### 1\. Decompress a Single File

```bash
gunzip file.txt.gz
```

- Output: `file.txt`
    
- Original `file.txt.gz` is removed after decompression.
    

### 2\. Keep the Compressed File

```bash
gunzip -k file.txt.gz
```

- Original `file.txt.gz` remains.
    
- Decompressed file: `file.txt`
    

### 3\. Write Decompressed Output to Standard Output

```bash
gunzip -c file.txt.gz > file.txt
```

- Compressed file remains.
    
- Output is redirected to `file.txt`.
    

### 4\. Decompress Multiple Files

```bash
gunzip file1.txt.gz file2.txt.gz
```

- Each file is decompressed to:
    
    - `file1.txt`, `file2.txt`

### 5\. Decompress Recursively in Directory

```bash
gunzip -r my_folder/
```

- Decompresses all `.gz` files inside `my_folder` and subdirectories.

### 6\. Test Compressed File Integrity

```bash
gunzip -t file.txt.gz
```

- Returns nothing if the file is valid.

### 7\. List Compressed File Information

```bash
gunzip -l file.txt.gz
```

- Example output:

```
         compressed   uncompressed  ratio uncompressed_name
               78              300  74.0% file.txt
```

### 8\. Force Decompression

```bash
gunzip -f file.txt.gz
```

- Decompress even if the target file already exists.

* * *

## Notes

- `gunzip` is simple and only works on `.gz` files.
    
- To decompress `.tar.gz` (or `.tgz`) archives, use `tar` with `-x`:
    

```bash
tar -xzvf archive.tar.gz
```

- `gunzip` is functionally the same as `gzip -d`:

```bash
gzip -d file.txt.gz
```

* * *

`gunzip` is the standard tool in Linux for decompressing `.gz` files and integrates seamlessly with other command-line utilities.

* * *

* * *

* * *

# `bzip2` Command in Linux

`bzip2` is a command-line utility for compressing files in Linux. It typically achieves higher compression ratios than `gzip`, producing files with the `.bz2` extension.

* * *

## Basic Syntax

```bash
bzip2 [options] filename
```

- `filename` – The file to compress.
    
- By default, `bzip2` replaces the original file with a compressed `.bz2` file.
    

* * *

## Common Options

| Option | Description |
| --- | --- |
| `-d` | Decompress the file (same as `bunzip2`) |
| `-k` | Keep the original file |
| `-z` | Compress (default behavior) |
| `-c` | Write output to stdout and keep original file |
| `-v` | Verbose mode, shows compression ratio |
| `-1` to `-9` | Compression level (`-1` fastest, `-9` best) |
| `-t` | Test compressed file integrity |
| `-r` | Recursively compress files in directories |

* * *

## Examples

### 1\. Compress a Single File

```bash
bzip2 file.txt
```

- Output: `file.txt.bz2`
    
- Original `file.txt` is removed.
    

### 2\. Keep the Original File

```bash
bzip2 -k file.txt
```

- Output: `file.txt.bz2`
    
- `file.txt` remains.
    

### 3\. Compress Multiple Files

```bash
bzip2 file1.txt file2.txt
```

- Each file is compressed individually:
    
    - `file1.txt.bz2`, `file2.txt.bz2`

### 4\. Compress Recursively in a Directory

```bash
bzip2 -r my_folder/
```

- Compresses all files inside `my_folder` and subdirectories.

### 5\. Verbose Compression

```bash
bzip2 -v file.txt
```

- Shows compression ratio:

```
file.txt: 72.5% -- replaced with file.txt.bz2
```

### 6\. Compress to Standard Output

```bash
bzip2 -c file.txt > file.txt.bz2
```

- Keeps original file, writes compressed output to `file.txt.bz2`

### 7\. Test Compressed File Integrity

```bash
bzip2 -t file.txt.bz2
```

- Returns nothing if the file is valid.

### 8\. Use Different Compression Levels

```bash
bzip2 -1 file.txt   # Fastest compression
bzip2 -9 file.txt   # Best compression
```

### 9\. Decompress File

```bash
bzip2 -d file.txt.bz2
```

- Or using `bunzip2`:

```bash
bunzip2 file.txt.bz2
```

- Restores `file.txt`.

* * *

## Notes

- `bzip2` is more efficient than `gzip` for compression but slower.
    
- To compress directories as a single file, combine with `tar`:
    

```bash
tar -cjvf archive.tar.bz2 folder_name/
```

- Decompress using:

```bash
tar -xjvf archive.tar.bz2
```

- `bzip2` only works on individual files; it doesn’t compress directories directly.

* * *

`bzip2` is a powerful Linux compression tool that balances good compression ratio with moderate speed, making it ideal for storage-efficient archiving.

* * *

* * *

# `bunzip2` Command in Linux

`bunzip2` is the command-line utility used to decompress files compressed with `bzip2`. It restores `.bz2` files to their original form.

* * *

## Basic Syntax

```bash
bunzip2 [options] filename.bz2
```

- `filename.bz2` – The compressed file to decompress.

> Note: `bunzip2` is equivalent to `bzip2 -d`.

* * *

## Common Options

| Option | Description |
| --- | --- |
| `-c` | Write output to stdout and keep original file |
| `-f` | Force decompression even if output file exists |
| `-k` | Keep the original compressed file |
| `-v` | Verbose mode, shows decompression process |
| `-t` | Test compressed file integrity |
| `-r` | Recursively decompress files in directories |

* * *

## Examples

### 1\. Decompress a Single File

```bash
bunzip2 file.txt.bz2
```

- Output: `file.txt`
    
- Original `file.txt.bz2` is removed.
    

### 2\. Keep the Compressed File

```bash
bunzip2 -k file.txt.bz2
```

- Output: `file.txt`
    
- Original `file.txt.bz2` remains.
    

### 3\. Write Decompressed Output to Standard Output

```bash
bunzip2 -c file.txt.bz2 > file.txt
```

- Original compressed file is kept.
    
- Output redirected to `file.txt`.
    

### 4\. Decompress Multiple Files

```bash
bunzip2 file1.txt.bz2 file2.txt.bz2
```

- Each file is decompressed to `file1.txt` and `file2.txt`.

### 5\. Decompress Recursively in Directory

```bash
bunzip2 -r my_folder/
```

- Decompresses all `.bz2` files inside `my_folder` and subdirectories.

### 6\. Test Compressed File Integrity

```bash
bunzip2 -t file.txt.bz2
```

- Returns nothing if the file is valid.

### 7\. Verbose Decompression

```bash
bunzip2 -v file.txt.bz2
```

- Shows progress and decompression info.

### 8\. Force Decompression

```bash
bunzip2 -f file.txt.bz2
```

- Decompress even if the output file exists.

* * *

## Notes

- `bunzip2` only works on `.bz2` files.
    
- To decompress `.tar.bz2` archives, use `tar` with `-xj`:
    

```bash
tar -xjvf archive.tar.bz2
```

- Functionally the same as `bzip2 -d`:

```bash
bzip2 -d file.txt.bz2
```

* * *

`bunzip2` is the standard Linux tool for decompressing `.bz2` files, complementing `bzip2` for compression tasks.

* * *

* * *

* * *

# `xz` Command in Linux

`xz` is a command-line utility for compressing files using the LZMA2 compression algorithm. It provides high compression ratios and creates files with the `.xz` extension.

* * *

## Basic Syntax

```bash
xz [options] filename
```

- `filename` – The file you want to compress.
    
- By default, `xz` replaces the original file with a compressed `.xz` file.
    

* * *

## Common Options

| Option | Description |
| --- | --- |
| `-d` | Decompress the file (same as `unxz`) |
| `-k` | Keep the original file |
| `-c` | Write output to stdout, keep original file |
| `-z` | Compress (default behavior) |
| `-v` | Verbose mode, shows compression ratio |
| `-1` to `-9` | Compression level (`-1` fastest, `-9` best) |
| `-t` | Test compressed file integrity |
| `-r` | Recursively compress files in directories |

* * *

## Examples

### 1\. Compress a Single File

```bash
xz file.txt
```

- Output: `file.txt.xz`
    
- Original `file.txt` is removed.
    

### 2\. Keep the Original File

```bash
xz -k file.txt
```

- Output: `file.txt.xz`
    
- Original `file.txt` remains.
    

### 3\. Compress Multiple Files

```bash
xz file1.txt file2.txt
```

- Each file is compressed individually:
    
    - `file1.txt.xz`, `file2.txt.xz`

### 4\. Compress Recursively in a Directory

```bash
xz -r my_folder/
```

- Compresses all files inside `my_folder` and subdirectories.

### 5\. Verbose Compression

```bash
xz -v file.txt
```

- Shows compression statistics.

### 6\. Compress to Standard Output

```bash
xz -c file.txt > file.txt.xz
```

- Original file is kept, compressed output written to `file.txt.xz`

### 7\. Test Compressed File Integrity

```bash
xz -t file.txt.xz
```

- Returns nothing if the file is valid.

### 8\. Use Different Compression Levels

```bash
xz -1 file.txt   # Fastest compression
xz -9 file.txt   # Best compression
```

### 9\. Decompress File

```bash
xz -d file.txt.xz
```

- Or using `unxz`:

```bash
unxz file.txt.xz
```

- Restores `file.txt`.

* * *

## Notes

- `xz` provides higher compression ratios than `gzip` and `bzip2`, but it is slower.
    
- To compress directories as a single file, use `tar` with `xz`:
    

```bash
tar -cJvf archive.tar.xz folder_name/
```

- Decompress using:

```bash
tar -xJvf archive.tar.xz
```

- `xz` is suitable for archival purposes when storage efficiency is a priority.

* * *

`xz` is a modern, efficient Linux compression tool ideal for high compression scenarios and complements `gzip` and `bzip2` utilities.

* * *

* * *

# `unxz` Command in Linux

`unxz` is the command-line utility used to decompress files compressed with `xz`. It restores `.xz` files to their original form.

* * *

## Basic Syntax

```bash
unxz [options] filename.xz
```

- `filename.xz` – The compressed file to decompress.

> Note: `unxz` is equivalent to `xz -d`.

* * *

## Common Options

| Option | Description |
| --- | --- |
| `-c` | Write output to stdout and keep original file |
| `-f` | Force decompression even if output file exists |
| `-k` | Keep the original compressed file |
| `-v` | Verbose mode, shows decompression process |
| `-t` | Test compressed file integrity |
| `-r` | Recursively decompress files in directories |

* * *

## Examples

### 1\. Decompress a Single File

```bash
unxz file.txt.xz
```

- Output: `file.txt`
    
- Original `file.txt.xz` is removed.
    

### 2\. Keep the Compressed File

```bash
unxz -k file.txt.xz
```

- Output: `file.txt`
    
- Original `file.txt.xz` remains.
    

### 3\. Write Decompressed Output to Standard Output

```bash
unxz -c file.txt.xz > file.txt
```

- Original compressed file is kept.
    
- Output redirected to `file.txt`.
    

### 4\. Decompress Multiple Files

```bash
unxz file1.txt.xz file2.txt.xz
```

- Each file is decompressed to `file1.txt` and `file2.txt`.

### 5\. Decompress Recursively in Directory

```bash
unxz -r my_folder/
```

- Decompresses all `.xz` files inside `my_folder` and subdirectories.

### 6\. Test Compressed File Integrity

```bash
unxz -t file.txt.xz
```

- Returns nothing if the file is valid.

### 7\. Verbose Decompression

```bash
unxz -v file.txt.xz
```

- Shows progress and decompression info.

### 8\. Force Decompression

```bash
unxz -f file.txt.xz
```

- Decompress even if the output file exists.

* * *

## Notes

- `unxz` only works on `.xz` files.
    
- To decompress `.tar.xz` archives, use `tar` with `-xJ`:
    

```bash
tar -xJvf archive.tar.xz
```

- Functionally the same as `xz -d`:

```bash
xz -d file.txt.xz
```

* * *

`unxz` is the standard Linux tool for decompressing `.xz` files, complementing `xz` for compression tasks.

* * *

* * *

* * *

# `zip` Command in Linux

`zip` is a command-line utility used to compress files and directories into a single `.zip` archive. It is widely used due to its compatibility with Windows and other operating systems.

* * *

## Basic Syntax

```bash
zip [options] archive_name.zip file1 file2 ...
```

- `archive_name.zip` – Name of the output archive.
    
- `file1 file2 ...` – Files or directories to include in the archive.
    

* * *

## Common Options

| Option | Description |
| --- | --- |
| `-r` | Recursively include directories |
| `-q` | Quiet mode, suppress output |
| `-v` | Verbose mode, show progress |
| `-e` | Encrypt the archive with a password |
| `-l` | Convert text files to Unix line endings |
| `-9` | Maximum compression level |

* * *

## Examples

### 1\. Compress Single File

```bash
zip file.zip file.txt
```

- Output: `file.zip` containing `file.txt`

### 2\. Compress Multiple Files

```bash
zip archive.zip file1.txt file2.txt file3.txt
```

- Output: `archive.zip` containing all three files.

### 3\. Compress a Directory Recursively

```bash
zip -r archive.zip my_folder/
```

- Output: `archive.zip` containing all files and subdirectories in `my_folder`.

### 4\. Maximum Compression Level

```bash
zip -9 archive.zip file.txt
```

- Uses highest compression.

### 5\. Encrypt Archive with Password

```bash
zip -e archive.zip file.txt
```

- Prompts for a password to secure the archive.

### 6\. Quiet Mode

```bash
zip -q archive.zip file.txt
```

- Compress without printing progress messages.

### 7\. Verbose Mode

```bash
zip -v archive.zip file.txt
```

- Shows compression statistics and progress.

* * *

## Decompressing Zip Files

- Use `unzip` to extract files:

```bash
unzip archive.zip
```

- Extract to a specific directory:

```bash
unzip archive.zip -d /path/to/destination/
```

- List contents without extracting:

```bash
unzip -l archive.zip
```

* * *

## Notes

- `.zip` is widely supported across Linux, Windows, and macOS.
    
- `zip` is ideal for creating single archives from multiple files or directories.
    
- For large files, `zip` may be slower than `gzip` or `xz`, but it offers better cross-platform compatibility.
    

* * *

`zip` is a simple and versatile tool to compress files and directories into a portable archive format, commonly used for sharing or backup purposes.

* * *

* * *

# `unzip` Command in Linux

`unzip` is a command-line utility used to extract files from `.zip` archives in Linux. It allows you to decompress files and directories archived using the `zip` command.

* * *

## Basic Syntax

```bash
unzip [options] archive_name.zip
```

- `archive_name.zip` – The `.zip` file to extract.

* * *

## Common Options

| Option | Description |
| --- | --- |
| `-d <dir>` | Extract files to a specific directory |
| `-l` | List archive contents without extracting |
| `-t` | Test archive integrity without extracting |
| `-o` | Overwrite existing files without prompting |
| `-n` | Never overwrite existing files |
| `-q` | Quiet mode, suppress output |
| `-v` | Verbose mode, show detailed info |

* * *

## Examples

### 1\. Extract All Files

```bash
unzip archive.zip
```

- Extracts all files in the current directory.

### 2\. Extract to a Specific Directory

```bash
unzip archive.zip -d /path/to/destination/
```

- Extracts files into the given directory.

### 3\. List Contents of a Zip File

```bash
unzip -l archive.zip
```

- Displays the list of files inside the archive without extracting.

### 4\. Test Archive Integrity

```bash
unzip -t archive.zip
```

- Checks if the archive is valid without extracting.

### 5\. Overwrite Existing Files Without Prompting

```bash
unzip -o archive.zip
```

- Automatically overwrites files if they exist.

### 6\. Never Overwrite Existing Files

```bash
unzip -n archive.zip
```

- Keeps existing files and skips extraction for them.

### 7\. Quiet Mode

```bash
unzip -q archive.zip
```

- Extracts files without showing progress messages.

### 8\. Verbose Mode

```bash
unzip -v archive.zip
```

- Shows detailed extraction info.

* * *

## Notes

- `unzip` works only with `.zip` files.
    
- For password-protected archives:
    

```bash
unzip -P password archive.zip
```

- Commonly used in Linux, Windows, and macOS for cross-platform archive extraction.
    
- For creating `.zip` files, use the `zip` command.
    

* * *

`unzip` is the standard tool for extracting `.zip` files in Linux, providing flexible options for extraction, testing, and directory management.

* * *

* * *

* * *

# `zstd` Command in Linux

`zstd` (Zstandard) is a fast compression utility for Linux that provides high compression ratios and extremely fast compression and decompression speeds. Compressed files typically use the `.zst` extension.

* * *

## Basic Syntax

```bash
zstd [options] filename
```

- `filename` – The file to compress.
    
- By default, `zstd` replaces the original file with a `.zst` compressed file.
    

* * *

## Common Options

| Option | Description |
| --- | --- |
| `-d` | Decompress the file (same as `unzstd`) |
| `-k` | Keep the original file |
| `-c` | Write output to stdout and keep original file |
| `-q` | Quiet mode, suppress output |
| `-v` | Verbose mode, show compression ratio |
| `-1` to `-19` | Compression levels (1 = fastest, 19 = best) |
| `-r` | Recursively compress files in directories |
| `--rm` | Remove input files after compression |
| `-t` | Test compressed file integrity |

* * *

## Examples

### 1\. Compress a Single File

```bash
zstd file.txt
```

- Output: `file.txt.zst`
    
- Original `file.txt` is removed.
    

### 2\. Keep the Original File

```bash
zstd -k file.txt
```

- Output: `file.txt.zst`
    
- Original file remains.
    

### 3\. Compress Multiple Files

```bash
zstd file1.txt file2.txt
```

- Each file is compressed individually:
    
    - `file1.txt.zst`, `file2.txt.zst`

### 4\. Compress Recursively in a Directory

```bash
zstd -r my_folder/
```

- Compresses all files inside `my_folder` and subdirectories.

### 5\. Use Different Compression Levels

```bash
zstd -1 file.txt   # Fastest compression
zstd -19 file.txt  # Maximum compression
```

### 6\. Compress to Standard Output

```bash
zstd -c file.txt > file.txt.zst
```

- Original file is kept.

### 7\. Decompress a File

```bash
zstd -d file.txt.zst
```

- Or using `unzstd`:

```bash
unzstd file.txt.zst
```

- Restores `file.txt`.

### 8\. Test Compressed File Integrity

```bash
zstd -t file.txt.zst
```

- Returns nothing if the file is valid.

* * *

## Notes

- `zstd` is faster than `gzip`, `bzip2`, and `xz` for compression and decompression.
    
- For directories, combine with `tar`:
    

```bash
tar -cf - my_folder/ | zstd -o archive.tar.zst
```

- Decompress with:

```bash
zstd -d archive.tar.zst -o archive.tar
```

- Highly configurable compression levels (1–19) allow balancing speed and size.

* * *

`zstd` is ideal for modern Linux systems requiring fast and efficient compression for storage, backups, and transfers.

* * *

* * *

* * *

&nbsp;
