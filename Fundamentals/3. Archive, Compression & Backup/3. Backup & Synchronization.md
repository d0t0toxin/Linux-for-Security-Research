* * *

**Backup & Synchronization:** `rsync`, `rdiff-backup`, `borg`, `duplicity`

* * *

* * *

# `Rsync` Command in Linux

`rsync` is a powerful Linux utility for efficiently transferring and synchronizing files between two locations over a local or remote system. It is widely used for backups, mirroring, and file transfer.

* * *

## Basic Syntax

```bash
rsync [options] source destination
```

- **source**: File or directory to copy.
    
- **destination**: Target location (local path or remote `user@host:/path`).
    

* * *

## Common Options

| Option | Description |
| --- | --- |
| `-a` | Archive mode; preserves permissions, symbolic links, timestamps, etc. |
| `-v` | Verbose output. |
| `-z` | Compress file data during transfer. |
| `-P` | Shows progress and keeps partially transferred files. |
| `--delete` | Delete files in the destination that are not in the source. |
| `-r` | Recursive; copy directories recursively. |
| `-u` | Skip files that are newer in the destination. |
| `-h` | Human-readable output. |
| `-e` | Specify remote shell (e.g., SSH). |

* * *

## Examples

### 1\. Copy files locally

```bash
rsync -av /source/directory/ /destination/directory/
```

### 2\. Copy files to a remote server

```bash
rsync -av /local/dir/ user@remotehost:/remote/dir/
```

### 3\. Copy files from a remote server

```bash
rsync -av user@remotehost:/remote/dir/ /local/dir/
```

### 4\. Show progress while copying

```bash
rsync -avP /source/ /destination/
```

### 5\. Sync directories and delete extra files in destination

```bash
rsync -av --delete /source/ /destination/
```

### 6\. Copy files over SSH with custom port

```bash
rsync -av -e "ssh -p 2222" /local/dir/ user@remotehost:/remote/dir/
```

### 7\. Exclude certain files or directories

```bash
rsync -av --exclude='*.log' /source/ /destination/
```

### 8\. Dry run (simulate sync without making changes)

```bash
rsync -av --dry-run /source/ /destination/
```

### 9\. Synchronize only updated files

```bash
rsync -avu /source/ /destination/
```

### 10\. Copy files with compression for faster transfer

```bash
rsync -avz /source/ user@remotehost:/destination/
```

* * *

## Advanced Usage

### 1\. Using `rsync` as a backup tool

```bash
rsync -av --delete /home/user/ /mnt/backup/user_backup/
```

### 2\. Mirroring a website directory to a remote server

```bash
rsync -avz --delete /var/www/html/ user@remotehost:/var/www/html/
```

### 3\. Synchronize large files efficiently

```bash
rsync -av --progress --partial /source/largefile.zip /destination/
```

### 4\. Combining multiple options

```bash
rsync -avzPh --exclude='*.tmp' /source/ user@remotehost:/destination/
```

* * *

Rsync is extremely flexible and has many more options. For full documentation, check the man page:

```bash
man rsync
```

* * *

* * *

# `rdiff-backup` Command in Linux

`rdiff-backup` is a powerful backup tool for Linux that combines the features of a mirror and an incremental backup. It allows you to keep a full backup of a directory while storing incremental changes, so you can restore files from any point in time.

* * *

## Basic Syntax

```bash
rdiff-backup [options] source_directory destination_directory
```

- **source_directory**: Directory you want to back up.
    
- **destination_directory**: Backup location (can be local or remote via SSH).
    

* * *

## Common Options

| Option | Description |
| --- | --- |
| `--remote-schema` | Specify how to connect to a remote host (e.g., `ssh %s@%h:%p`). |
| `--print-statistics` | Print summary statistics after backup. |
| `--list-increments` | List all available increments in a backup. |
| `--restore-as-of now` | Restore backup to the latest state. |
| `--remove-older-than` | Remove increments older than a certain time. |
| `--verify` | Verify the integrity of the backup. |
| `--exclude` | Exclude files or directories from backup. |
| `--include` | Include specific files or directories in backup. |
| `--verbosity` | Set verbosity level (0–5). |

* * *

## Examples

### 1\. Basic local backup

```bash
rdiff-backup /home/user /mnt/backup/user_backup
```

### 2\. Backup to a remote server via SSH

```bash
rdiff-backup /home/user user@remotehost::/backup/user_backup
```

### 3\. Restore the latest backup

```bash
rdiff-backup -r now /mnt/backup/user_backup/ /home/user_restore
```

### 4\. Restore to a specific date/time

```bash
rdiff-backup -r 2D /mnt/backup/user_backup/ /home/user_restore
```

> `2D` means 2 days ago. You can also use `2W` (weeks), `3M` (months), or full timestamps like `2026-01-20T10:00:00`

### 5\. List all increments in a backup

```bash
rdiff-backup --list-increments /mnt/backup/user_backup
```

### 6\. Remove backups older than 30 days

```bash
rdiff-backup --remove-older-than 30D /mnt/backup/user_backup
```

### 7\. Exclude files or directories from backup

```bash
rdiff-backup --exclude /home/user/cache /home/user /mnt/backup/user_backup
```

### 8\. Include only specific files or directories

```bash
rdiff-backup --include /home/user/Documents --exclude '**' /home/user /mnt/backup/user_backup
```

### 9\. Verify backup integrity

```bash
rdiff-backup --verify /mnt/backup/user_backup
```

### 10\. Backup with compression over SSH

```bash
rdiff-backup --remote-schema 'ssh -C %s@%h %s' /home/user user@remotehost::/backup/user_backup
```

* * *

## Advanced Usage

### 1\. Automating backup with cron

```bash
0 2 * * * /usr/bin/rdiff-backup /home/user /mnt/backup/user_backup
```

This runs the backup every day at 2 AM.

### 2\. Incremental backup rotation

- Use `--remove-older-than` to maintain a set retention period, e.g., daily backups for 7 days, weekly backups for 4 weeks.

### 3\. Combining options

```bash
rdiff-backup --exclude /home/user/tmp --print-statistics /home/user /mnt/backup/user_backup
```

* * *

Rdiff-backup is ideal for efficient incremental backups while keeping full historical copies. For complete options, check the man page:

```bash
man rdiff-backup
```

* * *

* * *

# `BorgBackup` (`borg`) Command in Linux

`borg` (BorgBackup) is a deduplicating backup program that is secure, efficient, and supports compression and encryption. It allows you to create backups locally or on remote servers and is ideal for long-term, incremental backups.

* * *

## Installation

On Debian/Ubuntu:

```bash
sudo apt install borgbackup
```

On RedHat/CentOS:

```bash
sudo yum install borgbackup
```

On Fedora:

```bash
sudo dnf install borgbackup
```

* * *

## Basic Concepts

- **Repository**: Where backups are stored.
    
- **Archive**: A snapshot of a directory at a certain point in time.
    
- **Pruning**: Removing old archives based on retention policies.
    
- **Deduplication**: Only unique chunks of data are stored, saving space.
    

* * *

## Basic Syntax

```bash
borg [command] [options] repository [archive] [paths]
```

- **repository**: Path to local or remote backup repository.
    
- **archive**: Name of the archive (backup snapshot).
    
- **paths**: Directories or files to back up.
    

* * *

## Common Commands and Options

| Command | Description |
| --- | --- |
| `borg init` | Initialize a new repository. |
| `borg create` | Create a new backup archive. |
| `borg list` | List archives in a repository. |
| `borg extract` | Restore files from an archive. |
| `borg info` | Show information about a repository or archive. |
| `borg prune` | Remove old archives based on retention policy. |
| `--compression` | Apply compression (`lz4`, `zstd`, `bz2`). |
| `--stats` | Show statistics after backup. |
| `--progress` | Show progress while backing up. |
| `--remote-path` | Use a specific borg path on remote hosts. |

* * *

## Examples

### 1\. Initialize a local repository

```bash
borg init --encryption=repokey /mnt/backup/borg_repo
```

### 2\. Create a backup archive

```bash
borg create --stats --progress /mnt/backup/borg_repo::my_backup_2026-01-20 /home/user
```

### 3\. List all archives

```bash
borg list /mnt/backup/borg_repo
```

### 4\. Extract a backup

```bash
borg extract /mnt/backup/borg_repo::my_backup_2026-01-20
```

### 5\. Show archive info

```bash
borg info /mnt/backup/borg_repo::my_backup_2026-01-20
```

### 6\. Prune old backups (keep last 7 daily, 4 weekly, 6 monthly)

```bash
borg prune -v --list /mnt/backup/borg_repo --keep-daily=7 --keep-weekly=4 --keep-monthly=6
```

### 7\. Backup to a remote server over SSH

```bash
borg create --stats --progress user@remotehost:/path/to/borg_repo::my_backup_2026-01-20 /home/user
```

### 8\. Backup with compression

```bash
borg create --compression zstd,9 /mnt/backup/borg_repo::my_backup_2026-01-20 /home/user
```

### 9\. Dry run (simulate backup)

```bash
borg create --dry-run /mnt/backup/borg_repo::my_backup_test /home/user
```

* * *

## Advanced Usage

### 1\. Automating backups with cron

```bash
0 2 * * * borg create --stats /mnt/backup/borg_repo::`date +\%Y-\%m-\%d` /home/user
```

### 2\. Encrypting backups

- `--encryption=repokey` stores the key in the repository.
    
- `--encryption=keyfile` stores the key in a separate file.
    

### 3\. Efficient remote backups

- Borg only transfers changed blocks, making incremental backups fast.

### 4\. Combining options

```bash
borg create --compression zstd,9 --stats --progress /mnt/backup/borg_repo::daily_backup /home/user --exclude /home/user/tmp
```

* * *

BorgBackup is a robust, secure, and space-efficient solution for daily or long-term backups. For full documentation:

```bash
man borg
```

* * *

* * *

# `Duplicity` Command in Linux

`duplicity` is a backup tool that creates encrypted, incremental backups to local or remote storage. It supports full and incremental backups, encryption via GPG, and various protocols like SSH, FTP, WebDAV, and cloud storage.

* * *

## Installation

On Debian/Ubuntu:

```bash
sudo apt install duplicity
```

On RedHat/CentOS:

```bash
sudo yum install duplicity
```

On Fedora:

```bash
sudo dnf install duplicity
```

* * *

## Basic Concepts

- **Source directory**: Directory to back up.
    
- **Target URL**: Backup location (local path or remote URL, e.g., `scp://user@host//path`).
    
- **Full backup**: A complete snapshot of the source.
    
- **Incremental backup**: Only changes since the last backup.
    
- **Encryption**: Uses GPG for secure backups.
    

* * *

## Basic Syntax

```bash
duplicity [options] source_url target_url
```

- **source_url**: Local directory to back up.
    
- **target_url**: Local or remote backup location.
    

* * *

## Common Options

| Option | Description |
| --- | --- |
| `--encrypt-key` | GPG key ID for encrypting backups. |
| `--sign-key` | GPG key ID for signing backups. |
| `--full-if-older-than` | Create a new full backup if the last full is older than specified time. |
| `--no-encryption` | Backup without encryption. |
| `--include` | Include specific files or directories. |
| `--exclude` | Exclude files or directories. |
| `--progress` | Show progress during backup. |
| `--restore-time` | Restore files as of a specific date/time. |
| `--remove-older-than` | Remove backups older than a specified date. |
| `--list-current-files` | List all files in the latest backup. |

* * *

## Examples

### 1\. Create a local backup

```bash
duplicity /home/user file:///mnt/backup/duplicity
```

### 2\. Create a remote backup via SSH

```bash
duplicity /home/user scp://user@remotehost//backup/duplicity
```

### 3\. Create an encrypted backup

```bash
duplicity --encrypt-key ABCDEF12 /home/user scp://user@remotehost//backup/duplicity
```

### 4\. Restore latest backup

```bash
duplicity restore scp://user@remotehost//backup/duplicity /home/user_restore
```

### 5\. Restore to a specific date/time

```bash
duplicity restore --restore-time 2026-01-15 scp://user@remotehost//backup/duplicity /home/user_restore
```

### 6\. Incremental backup after a full backup

```bash
duplicity incremental /home/user scp://user@remotehost//backup/duplicity
```

### 7\. Remove old backups older than 30 days

```bash
duplicity remove-older-than 30D --force scp://user@remotehost//backup/duplicity
```

### 8\. List current files in backup

```bash
duplicity list-current-files scp://user@remotehost//backup/duplicity
```

### 9\. Backup with include/exclude patterns

```bash
duplicity --exclude /home/user/tmp --include /home/user/Documents /home/user scp://user@remotehost//backup/duplicity
```

### 10\. Full backup if older than 7 days

```bash
duplicity --full-if-older-than 7D /home/user scp://user@remotehost//backup/duplicity
```

* * *

## Advanced Usage

### 1\. Automate backups with cron

```bash
0 3 * * * duplicity --full-if-older-than 7D /home/user scp://user@remotehost//backup/duplicity
```

This runs the backup every day at 3 AM and creates a full backup if the last one is older than 7 days.

### 2\. Using Duplicity with cloud storage

- Supports Amazon S3, Google Drive, Backblaze B2, etc.
    
- Example for Amazon S3:
    

```bash
export AWS_ACCESS_KEY_ID="your-access-key"
export AWS_SECRET_ACCESS_KEY="your-secret-key"
duplicity /home/user s3://bucket-name/backup
```

### 3\. Encrypting and signing backups

```bash
duplicity --encrypt-key ABCDEF12 --sign-key 12345678 /home/user scp://user@remotehost//backup/duplicity
```

* * *

Duplicity is ideal for secure, incremental, and automated backups to local or remote locations. For complete documentation:

```bash
man duplicity
```

* * *

* * *

&nbsp;
