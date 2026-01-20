* * *

Searching: `grep` `egrep/grep -E` `fgrep/grep -F` `find` `locate` `which` `whereis` `type` `apropos`

* * *

* * *

**What is a Regular Expression?**  
A regex is a pattern that describes a set of strings.  
Example: `^root` --> Match any line that starts with root.

* * *

**Linux mainly uses two regex flavors:**  
**Basic Regular Expressions (BRE)**: Used by default in `grep` `sed`  
**Extended Regular Expressions (ERE)**: `grep -E` `egrep` `awk`

* * *

Not every command in linux is capable of regex.  
These are the list of commands that are capable of regex:

```bash
grep
grep -E / egrep
sed
awk
find -regex
```

* * *

**Common Regex Symbols:**  
Symbol Meaning Example  
`.` --> any single character --> `a.c`  
`^` --> start of line --> `^root`  
`$` --> end of line --> `bash$`  
`*` --> 0 or more --> `lo*`  
`+` --> 1 or more --> `lo+`  
`?` --> 0 or 1 --> `colou?r`  
`{n}` --> Exactly n times --> `a{3}`  
`{n,m}` --> Between n and m --> `a{2,4}`  
`[abc]` --> Any one of a,b,c --> `[aeiou]`  
`[^abc` --> Not a,b,c --> `[^0-9]`  
`()` --> grouping --> `(ab)+`  
`\` --> Escape Sequence --> `\.`

* * *

**Character Classes**  
Classes Matches  
`[0-9]` --> digits  
`[a-z]` --> lowercase  
`[A-Z]` --> uppercase  
`[a-zA-Z` --> letters  
`[[:digit:]]` --> digits  
`[[:alpha:]]` --> letters  
`[[:alnum:]]` --> letters and numbers  
`[[:space:]]` --> whitespace

* * *

* * *

# GREP Command in Linux

`grep` stands for **Global Regular Expression Print**. It’s used to **search for patterns in files or input streams**.

* * *

## 1\. Basic Syntax

```bash
grep [OPTIONS] PATTERN [FILE...]
```

- **PATTERN** → The text or regex pattern you want to search.
    
- **FILE** → The file(s) you want to search in.
    
- If no file is provided, `grep` reads from **stdin**.
    

**Example:**

```bash
grep "hello" file.txt
```

* * *

## 2\. Simple Examples

```bash
grep "error" logfile.txt
cat logfile.txt | grep "error"
grep "404" *.log
```

* * *

## 3\. Commonly Used Options

| Option | Description | Example |
| --- | --- | --- |
| `-i` | Ignore case | `grep -i "error" file.txt` |
| `-v` | Invert match | `grep -v "pass" file.txt` |
| `-c` | Count matching lines | `grep -c "error" file.txt` |
| `-n` | Show line numbers | `grep -n "error" file.txt` |
| `-l` | List filenames with matches | `grep -l "error" *.log` |
| `-L` | List filenames without matches | `grep -L "error" *.log` |
| `-r` or `-R` | Recursive search | `grep -r "TODO" ~/projects` |
| `-w` | Match whole words only | `grep -w "is" file.txt` |
| `-x` | Match whole line exactly | `grep -x "hello world" file.txt` |
| `-o` | Only print matched part | `grep -o "[0-9]+" file.txt` |
| `-q` | Quiet mode | `grep -q "error" file.txt` |
| `-F` / `fgrep` | Fixed string search (no regex, faster) | `grep -F "exact string" file.txt` or `fgrep "exact string" file.txt` |

* * *

## 4\. Regular Expressions

### 4.1 Basic Regular Expressions (BRE)

| Regex | Meaning | Example |
| --- | --- | --- |
| `.` | Any single character | `grep "h.t" file.txt` |
| `*` | Zero or more of previous char | `grep "lo*l" file.txt` |
| `^` | Start of line | `grep "^Error" file.txt` |
| `$` | End of line | `grep "done$" file.txt` |
| `\` | Escape special character | `grep "\.txt" file.txt` |

### 4.2 Extended Regular Expressions (ERE)

Use `grep -E` or `egrep`:

| Regex | Meaning | Example |
| --- | --- | --- |
| `+` | One or more | `grep -E "lo+l" file.txt` |
| `?` | Zero or one | `grep -E "colou?r" file.txt` |
| \`  | \`  | OR operator |
| `()` | Grouping | \`grep -E "(error |

* * *

## 5\. Advanced Usage

### 5.1 Multiple Patterns

```bash
grep -E "error|fail|warning" logfile.txt
```

### 5.2 Show Context

```bash
grep -A 2 "error" file.txt  # after 2 lines
grep -B 2 "error" file.txt  # before 2 lines
grep -C 2 "error" file.txt  # 2 lines before & after
```

### 5.3 Compressed Files

```bash
zgrep "error" logfile.gz
```

### 5.4 Recursive Search with Filename

```bash
grep -rnw "/path/to/dir" -e "TODO"
```

### 5.5 Using grep with Pipes

```bash
dmesg | grep -i "usb"
ps aux | grep firefox
```

* * *

## 6\. Performance Tips

- For very large files or exact string search (no regex):

```bash
grep -F "exact string" bigfile.txt
fgrep "exact string" bigfile.txt
```

- Exclude files or directories:

```bash
grep -R --exclude="*.log" "error" ~/projects
grep -R --exclude-dir="node_modules" "TODO" ~/projects
```

* * *

## 7\. Exit Status

- `0` → Match found
    
- `1` → No match
    
- `2` → Error
    

Example in scripts:

```bash
if grep -q "error" logfile.txt; then
    echo "Error found!"
fi
```

* * *

## 8\. Combining grep with Other Tools

```bash
grep "pattern" file.txt | awk '{print $2}'
grep "pattern" file.txt | sed 's/foo/bar/'
find . -type f -name "*.txt" -exec grep "pattern" {} \;
```

* * *

## 9\. Visualization Example

```
File: test.txt
1: Hello world
2: Error: file not found
3: Warning: low disk space
4: All operations done
```

- `grep "Error" test.txt` → Line 2
    
- `grep -i "error" test.txt` → Line 2
    
- `grep -n "Error" test.txt` → `2:Error: file not found`
    
- `grep -C 1 "Error" test.txt` → Shows lines 1–3
    

* * *

* * *

# FIND Command in Linux

`find` is used to **search for files and directories** in a directory hierarchy based on **name, type, size, time, permissions, and more**.

* * *

## 1\. Basic Syntax

```bash
find [PATH] [OPTIONS] [EXPRESSION]
```

- **PATH** → Directory where search starts (default is current directory `.`)
    
- **OPTIONS / EXPRESSION** → Criteria to match files or directories
    
- Multiple criteria can be combined using `-and` (default), `-or`, `!` (not)
    

**Example:**

```bash
find /home/user -name "*.txt"
```

* * *

## 2\. Commonly Used Options

| Option | Description | Example |
| --- | --- | --- |
| `-name` | Search by filename (case-sensitive) | `find . -name "file.txt"` |
| `-iname` | Case-insensitive filename | `find . -iname "file.TXT"` |
| `-type` | Search by type: `f`\=file, `d`\=directory, `l`\=symlink | `find . -type d` |
| `-size` | Search by size: `+n` = greater, `-n` = less, `n`\=exact | `find . -size +1M` |
| `-mtime` | Modified n days ago: `+n`\=older, `-n`\=newer | `find . -mtime -7` |
| `-atime` | Accessed n days ago | `find . -atime -1` |
| `-ctime` | Changed metadata n days ago | `find . -ctime +30` |
| `-perm` | Search by permissions | `find . -perm 644` |
| `-user` | Owned by user | `find . -user sushil` |
| `-group` | Owned by group | `find . -group admin` |
| `-empty` | Empty file or directory | `find . -empty` |
| `-maxdepth` | Limit search depth | `find . -maxdepth 1 -name "*.txt"` |
| `-mindepth` | Minimum depth | `find . -mindepth 2 -name "*.txt"` |

* * *

## 3\. Combining Expressions

- **AND (default)**:

```bash
find . -type f -name "*.txt"
```

- **OR**:

```bash
find . -type f -name "*.txt" -or -name "*.log"
```

- **NOT**:

```bash
find . -type f ! -name "*.txt"
```

- **Grouping with parentheses (escaped)**:

```bash
find . \( -name "*.txt" -or -name "*.log" \) -type f
```

* * *

## 4\. Actions

| Action | Description | Example |
| --- | --- | --- |
| `-print` | Print path (default) | `find . -name "*.txt" -print` |
| `-ls` | Detailed listing | `find . -name "*.txt" -ls` |
| `-exec` | Execute command on each match | `find . -name "*.txt" -exec cat {} \;` |
| `-ok` | Like `-exec` but asks confirmation | `find . -name "*.txt" -ok rm {} \;` |
| `-delete` | Delete matching files | `find . -type f -name "*.tmp" -delete` |

**Tips for `-exec`**:

- Use `+` instead of `\;` to pass multiple files at once:

```bash
find . -name "*.txt" -exec cat {} +
```

* * *

## 5\. Examples

### 5.1 Find by name

```bash
find /var/log -name "syslog"
```

### 5.2 Case-insensitive

```bash
find . -iname "README.md"
```

### 5.3 Find directories

```bash
find /home -type d -name "Downloads"
```

### 5.4 Find files larger than 100MB

```bash
find / -type f -size +100M
```

### 5.5 Find files modified in last 7 days

```bash
find ~/projects -type f -mtime -7
```

### 5.6 Delete temporary files

```bash
find . -type f -name "*.tmp" -delete
```

### 5.7 Execute command on files

```bash
find . -type f -name "*.log" -exec gzip {} \;
```

### 5.8 Find files by permission

```bash
find . -type f -perm 644
find . -type f ! -perm 644
```

### 5.9 Combining criteria

```bash
find . -type f -name "*.log" -size +10M -mtime -30
```

* * *

## 6\. Performance Tips

1.  Use `-maxdepth`/`-mindepth` to limit recursion.
    
2.  Avoid `sudo find /` if not necessary.
    
3.  Use `-prune` to exclude directories:
    

```bash
find . -path "./node_modules" -prune -o -name "*.js" -print
```

4.  `locate` is faster for simple filename searches if database is updated.

* * *

## 7\. Exit Status

- `0` → Matches found
    
- `1` → No matches
    
- `2` → Syntax error
    

* * *

## 8\. Combining with Other Commands

```bash
find . -type f -name "*.log" -exec grep "ERROR" {} \;
find . -type f -name "*.sh" | xargs chmod +x
```

- `xargs` helps run commands faster than `-exec` with `\;`.

* * *

* * *

# LOCATE Command in Linux

`locate` is used to **quickly find files and directories** by name using a **prebuilt database**, unlike `find`, which searches in real-time. It is **much faster**, especially for large file systems.

* * *

## 1\. Basic Syntax

```bash
locate [OPTIONS] PATTERN
```

- **PATTERN** → File or directory name to search
    
- Uses a **database** usually updated with `updatedb`
    
- Searches are case-sensitive by default
    

**Example:**

```bash
locate file.txt
```

* * *

## 2\. Commonly Used Options

| Option | Description | Example |
| --- | --- | --- |
| `-i` | Case-insensitive search | `locate -i readme.md` |
| `-c` | Count number of matches | `locate -c "*.log"` |
| `-n N` | Show only first N matches | `locate -n 10 "*.conf"` |
| `-r` | Use regex pattern | `locate -r '.*\.txt$'` |
| `-e` | Only existing files | `locate -e "*.sh"` |
| `--limit=N` | Limit number of results | `locate --limit=5 "*.jpg"` |

* * *

## 3\. Updating the Database

`locate` relies on a **database** that may not include recently created files. Update it using:

```bash
sudo updatedb
```

- Typically run automatically via cron daily
    
- Can use options like `--prune` to exclude directories
    

```bash
sudo updatedb --prune=/tmp
```

* * *

## 4\. Examples

### 4.1 Basic search

```bash
locate file.txt
```

### 4.2 Case-insensitive search

```bash
locate -i README.md
```

### 4.3 Count matching files

```bash
locate -c '*.log'
```

### 4.4 Limit number of results

```bash
locate -n 10 '*.conf'
```

### 4.5 Use regex

```bash
locate -r '.*\.txt$'
```

### 4.6 Only existing files

```bash
locate -e '*.sh'
```

### 4.7 Combine with `grep` for filtering

```bash
locate '*.log' | grep 'error'
```

### 4.8 Exclude directories from database

```bash
sudo updatedb --prune=/var/cache
```

* * *

## 5\. Tips

1.  Use `updatedb` regularly if you rely on `locate` for recent files.
    
2.  `locate` is **faster than `find`** for known filenames.
    
3.  Combine `locate` with `grep` for more refined searches.
    
4.  The database may not include **very new files** until updated.
    
5.  Useful in scripts for quick existence checks:
    

```bash
if locate -e 'important.conf' > /dev/null; then
    echo "File exists!"
fi
```

* * *

* * *

# WHICH Command in Linux

The `which` command **shows the full path of executables** that would run if you type a command in the shell. It searches the directories listed in your `$PATH`.

* * *

## 1\. Basic Syntax

```bash
which [OPTIONS] COMMAND
```

- **COMMAND** → The program you want the path for.

**Example:**

```bash
which python3
```

Output:

```
/usr/bin/python3
```

* * *

## 2\. Common Options

| Option | Description | Example |
| --- | --- | --- |
| `-a` | Show **all matches** in `$PATH` | `which -a python3` |
| `--skip-dot` | Skip current directory `.` in search | `which --skip-dot ls` |
| `-s` | Silent mode (returns exit status only) | `which -s git && echo "Git installed"` |

* * *

## 3\. Examples

### 3.1 Find a single executable

```bash
which ls
# /bin/ls
```

### 3.2 Find all locations of an executable

```bash
which -a python
# /usr/bin/python
# /usr/local/bin/python
```

### 3.3 Check if a command exists

```bash
if which git > /dev/null; then
    echo "Git is installed"
else
    echo "Git is not installed"
fi
```

### 3.4 Combining with other commands

```bash
which -a node | xargs -n1 ls -l
```

* * *

## 4\. Notes

1.  `which` only works for commands in your `$PATH`.
    
2.  It does **not detect shell aliases**. Use `type COMMAND` for that.
    
3.  Useful for **debugging PATH issues**.
    
4.  Fast way to verify which executable will run for a command.
    

* * *

* * *

# WHEREIS Command in Linux

The `whereis` command **locates the binary, source, and manual page files** for a command. Unlike `which`, it searches **standard system directories**, not just `$PATH`.

* * *

## 1\. Basic Syntax

```bash
whereis [OPTIONS] COMMAND
```

- **COMMAND** → The program you want to locate.
    
- Returns paths for:
    
    - Binary executable
        
    - Source code (if installed)
        
    - Manual pages
        

**Example:**

```bash
whereis ls
```

Output:

```
ls: /bin/ls /usr/share/man/man1/ls.1.gz
```

- `/bin/ls` → binary
    
- `/usr/share/man/man1/ls.1.gz` → man page
    

* * *

## 2\. Commonly Used Options

| Option | Description | Example |
| --- | --- | --- |
| `-b` | Search **only for binaries** | `whereis -b gcc` |
| `-m` | Search **only for man pages** | `whereis -m ls` |
| `-s` | Search **only for source files** | `whereis -s grep` |
| `-u` | Search **for unusual files** | `whereis -u python` |
| `-B`, `-M`, `-S` | Specify custom directories to search | `whereis -B /usr/local/bin -M /usr/share/man ls` |

* * *

## 3\. Examples

### 3.1 Locate binary and man page

```bash
whereis ls
# ls: /bin/ls /usr/share/man/man1/ls.1.gz
```

### 3.2 Locate **only binary**

```bash
whereis -b python3
# python3: /usr/bin/python3
```

### 3.3 Locate **only man page**

```bash
whereis -m grep
# grep: /usr/share/man/man1/grep.1.gz
```

### 3.4 Locate **source files**

```bash
whereis -s bash
# bash: /usr/src/bash-5.1/bash.c
```

### 3.5 Locate **unusual files**

```bash
whereis -u vim
```

### 3.6 Custom search directories

```bash
whereis -B /usr/local/bin -M /usr/share/man ls
```

* * *

## 4\. Notes

1.  Searches **predefined system directories**, not `$PATH`.
    
2.  Faster than `find` because it searches limited locations.
    
3.  Useful for **system administration** to quickly locate executables, man pages, and source code.
    
4.  Can be combined with `grep`:
    

```bash
whereis gcc | grep bin
```

* * *

* * *

# TYPE Command in Linux

The `type` command **tells you how a command name is interpreted by the shell**. It can show if a command is a **builtin, alias, function, or executable**.

* * *

## 1\. Basic Syntax

```bash
type [OPTIONS] COMMAND
```

- **COMMAND** → The command or name to inspect.

**Example:**

```bash
type ls
```

Output:

```
ls is aliased to 'ls --color=auto'
```

```bash
type cd
```

Output:

```
cd is a shell builtin
```

* * *

## 2\. Common Options

| Option | Description | Example |
| --- | --- | --- |
| `-t` | Print **type only** | `type -t cd` → `builtin` |
| `-p` | Print **path** if executable | `type -p ls` → `/bin/ls` |
| `-a` | Show **all occurrences** | `type -a python` |
| `-f` | Suppress shell function lookup | `type -f myfunc` |

* * *

## 3\. Examples

### 3.1 Check if command is an alias or builtin

```bash
type cd
# cd is a shell builtin

type ll
# ll is aliased to 'ls -l'
```

### 3.2 Show type only

```bash
type -t cd
# builtin

type -t ls
# file
```

### 3.3 Show all locations

```bash
type -a python
# python is /usr/bin/python
# python is /usr/local/bin/python
```

### 3.4 Show path of executable

```bash
type -p gcc
# /usr/bin/gcc
```

### 3.5 Check shell functions

```bash
myfunc() { echo "Hello"; }
type myfunc
# myfunc is a function
```

* * *

## 4\. Notes

1.  `type` is a **shell built-in**, works faster than `which` for shell commands.
    
2.  Useful for **debugging aliases, functions, and PATH issues**.
    
3.  Can be used in scripts:
    

```bash
if [ "$(type -t mycmd)" = "file" ]; then
    echo "Executable exists"
fi
```

* * *

* * *

# APROPOS Command in Linux

The `apropos` command **searches manual page names and descriptions** for a keyword. It is useful when you **don’t know the exact command** but know its functionality.

* * *

## 1\. Basic Syntax

```bash
apropos [OPTIONS] KEYWORD
```

- **KEYWORD** → Term to search in man pages.

**Example:**

```bash
apropos network
```

Output:

```
ifconfig (8)       - configure network interfaces
netstat (8)        - Print network connections
ping (8)           - send ICMP ECHO_REQUEST to network hosts
```

* * *

## 2\. Common Options

| Option | Description | Example |
| --- | --- | --- |
| `-s SECTION` | Limit search to a man **section** | `apropos -s 8 mount` |
| `-r` | Use **regular expressions** | `apropos -r 'net.*'` |
| `-e` | Match **exact keyword** | `apropos -e ping` |
| `--exact` | Same as `-e` | `apropos --exact ls` |
| `--long` | Show **full descriptions** | `apropos --long network` |

* * *

## 3\. Examples

### 3.1 Search for commands related to network

```bash
apropos network
```

### 3.2 Search in a specific man section

```bash
apropos -s 1 mount
```

- Section 1 → user commands
    
- Section 8 → system/admin commands
    

### 3.3 Use regular expressions

```bash
apropos -r '^net'
```

### 3.4 Search for exact keyword

```bash
apropos -e ls
```

### 3.5 Combine with `grep` for filtering

```bash
apropos network | grep configure
```

* * *

## 4\. Notes

1.  `apropos` searches the **whatis database**, updated with `mandb` or `makewhatis`.
    
2.  Useful when you **forget a command name** but know its function.
    
3.  Faster than searching man pages manually.
    
4.  Can be combined with `man`:
    

```bash
man $(apropos mount | head -n 1 | awk '{print $1}')
```

* * *

* * *

&nbsp;
