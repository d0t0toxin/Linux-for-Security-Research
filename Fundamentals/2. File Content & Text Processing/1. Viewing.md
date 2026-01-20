* * *

Viewing: `cat` `tac` `more` `less` `head` `tail` `echo` `printf` `nl` `od` `strings` `watch` `view`  

* * *

* * *

* * *

The `cat` command in Linux, short for **concatenate**, is one of the most fundamental and versatile commands for working with files. While its primary function is to **display file contents**, it also serves in **file creation, combining files, and scripting tasks**.

1\. Concatenates and prints the contents of `file1.txt` followed by `file2.txt`.

```bash
>> cat file1.txt file2.txt
```

2\. `cat` can also create files and append contents interactively:

```bash
>> cat > newfile.txt
This is the test file!!! #ctrl+d to save and exit
>> cat >> newfile.txt 
This is the second line of the test file!!!                
>> cat newfile.txt 
This is the test file!!!This is the second line of the test file!!!
```

3\. `cat` can merge multiple files into one:

- `>` overwrites `merged.txt`.
- `>>` appends instead of overwriting.

```bash
>> cat file1.txt file2.txt > merged.txt
```

**Option Description**  
`-n` Number all output lines.   
`-b` Number only non-empty lines.  
`-s` Squeeze multiple blank lines into one.  
`-E` Show `$` at the end of each line.  
`-T` Show tabs as `^I`.  
`-A` Display all special characters. Equivalent to `-vET`.  
`-v` Show non-printing characters (except tabs/newlines).

4\. We can write multiple lines.

```bash
>> cat <<EOF > eof.txt
heredoc> This is the first string!!!
heredoc> This is the second string!!
heredoc> This is the third string!!
heredoc> EOF
                                                                                                                                                              
>> cat eof.txt        
This is the first string!!!
This is the second string!!
This is the third string!!
```

* * *

* * *

* * *

## 1\. What is `tac`?

`tac` is a standard Linux/Unix command that **prints files line by line in reverse order**.

- It is literally the reverse of `cat`
    
- Reads from **bottom to top**, not character by character
    

* * *

## 2\. Basic Syntax

```bash
tac [OPTION]... [FILE]...
```

- If **FILE** is not provided, `tac` reads from **standard input (stdin)**
    
- Multiple files are allowed
    

* * *

## 3\. Basic Examples

### Reverse a file

```bash
tac file.txt
```

### Compare with `cat`

```bash
cat file.txt   # top → bottom
tac file.txt   # bottom → top
```

### Multiple files

```bash
tac file1.txt file2.txt
```

> Output order: last line of `file2.txt` → first line of `file1.txt`

* * *

## 4\. Reading from Standard Input (Pipes)

`tac` is commonly used in pipelines.

### Reverse output of another command

```bash
dmesg | tac
```

### Reverse sorted output

```bash
sort names.txt | tac
```

* * *

## 5\. Options (Flags)

### 5.1 `-b`, `--before`

Print the separator **before** the record instead of after.

```bash
tac -b file.txt
```

Useful when working with custom separators.

* * *

### 5.2 `-r`, `--regex`

Treat the separator as a **regular expression**.

```bash
tac -r -s ':' file.txt
```

* * *

### 5.3 `-s`, `--separator=STRING`

Specify a **custom separator** instead of newline.

#### Example: Reverse paragraphs

```bash
tac -s '\n\n' file.txt
```

#### Example: Reverse colon-separated records

```bash
tac -s ':' file.txt
```

* * *

## 6\. Understanding Separators (Very Important)

By default:

- Separator = `\n` (newline)
    
- Each **line** is one record
    

### Custom separator behavior

| Separator | Meaning |
| --- | --- |
| `\n` | Reverse lines |
| `\n\n` | Reverse paragraphs |
| `:` | Reverse colon-separated fields |

* * *

## 7\. Advanced Usage

### 7.1 Reverse a log file

```bash
tac /var/log/syslog
```

### 7.2 View last errors first

```bash
tac error.log | less
```

### 7.3 Combine with `grep`

```bash
tac app.log | grep -m 1 "ERROR"
```

> Finds the **most recent** error first

* * *

* * *

* * *

Always prefer `less` unless `more` is explicitly required

## 1\. Introduction

`more` and `less` are pagers — commands used to view text output one screen at a time.

- Commonly used for logs, large files, and command output
    
- `less` is a powerful improvement over `more`
    

> Rule of thumb: Always prefer `less` unless `more` is explicitly required

* * *

## 2\. `more` Command

### 2.1 What is `more`?

`more` displays text page by page, allowing only forward navigation.

### 2.2 Syntax

```bash
more [OPTION] FILE
```

### 2.3 Basic Usage

```bash
more file.txt
```

### 2.4 Navigation Keys (`more`)

| Key | Action |
| --- | --- |
| Space | Next page |
| Enter | Next line |
| b   | Back one page (limited) |
| q   | Quit |
| /pattern | Search forward |

⚠️ Backward movement is very limited.

* * *

### 2.5 Pipe with `more`

```bash
ls -l /etc | more
```

* * *

### 2.6 Limitations of `more`

❌ Cannot scroll freely backward  
❌ No advanced search  
❌ No horizontal scrolling  
❌ Minimal customization

* * *

## 33.1 What is `less`?

`less` is an advanced pager that allows:

- Forward & backward scrolling
    
- Fast searching
    
- Better performance
    

> Name joke: *less is more powerful than more*

* * *

### 3.2 Syntax

```bash
less [OPTION] FILE
```

* * *

### 3.3 Basic Usage

```bash
less file.txt
```

Pipe example:

```bash
dmesg | less
```

* * *

## 4\. Navigation Keys (`less`)

### 4.1 Scrolling

| Key | Action |
| --- | --- |
| Space / f | Forward one page |
| b   | Backward one page |
| Enter | Forward one line |
| ↑ ↓ | Line up/down |
| g   | Go to start |
| G   | Go to end |

* * *

### 4.2 Searching

| Key | Action |
| --- | --- |
| /pattern | Search forward |
| ?pattern | Search backward |
| n   | Next match |
| N   | Previous match |

* * *

### 4.3 Other Useful Keys

| Key | Action |
| --- | --- |
| q   | Quit |
| h   | Help |
| \=  | File info |
| v   | Edit file in editor |

* * *

5\. Important `less` Options

### 5.1 `-N` — Show line numbers

```bash
less -N file.txt
```

* * *

### 5.2 `-S` — Disable line wrapping

```bash
less -S widefile.txt
```

> Enables horizontal scrolling

* * *

### 5.3 `+F` — Follow file (like `tail -f`)

```bash
less +F logfile.log
```

Press `Ctrl+C` to stop following.

* * *

### 5.4 `-X` — Do not clear screen on exit

```bash
less -X file.txt
```

* * *

6\. `less` with Logs (Real-World)

### View log safely

```bash
less /var/log/syslog
```

### Jump to latest logs

```bash
less +G logfile.log
```

### Search last error

```bash
tac logfile.log | less
```

* * *

7\. `more` vs `less`

| Feature | more | less |
| --- | --- | --- |
| Forward scrolling | ✅   | ✅   |
| Backward scrolling | ❌   | ✅   |
| Search backward | ❌   | ✅   |
| Large file handling | ❌   | ✅   |
| Follow file | ❌   | ✅   |
| Default pager | Old | Modern |

* * *

## 8\. Environment Variables

`PAGER`

```bash
export PAGER=less
```

Controls default pager for many commands (`man`, `git`, etc.)

* * *

* * *

* * *

# `head` and `tail` Commands in Linux

## 1\. Introduction

`head` and `tail` are core Linux commands used to **view the beginning or end of files**.

- `head` → shows the **first part** of a file
    
- `tail` → shows the **last part** of a file
    

Widely used for:

- Logs
    
- Configuration files
    
- Data inspection
    
- Scripting and pipelines
    

* * *

## 2\. `head` Command

### 2.1 What is `head`?

`head` outputs the **first 10 lines** of a file by default.

### 2.2 Syntax

```bash
head [OPTION]... [FILE]...
```

* * *

### 2.3 Basic Usage

```bash
head file.txt
```

* * *

### 2.4 Common Options

#### `-n` — Number of lines

```bash
head -n 5 file.txt
```

#### `-c` — Number of bytes

```bash
head -c 20 file.txt
```

* * *

### 2.5 Multiple Files

```bash
head file1.txt file2.txt
```

* * *

## 3\. `tail` Command

### 3.1 What is `tail`?

`tail` outputs the **last 10 lines** of a file by default.

### 3.2 Syntax

```bash
tail [OPTION]... [FILE]...
```

* * *

### 3.3 Basic Usage

```bash
tail file.txt
```

* * *

### 3.4 Common Options

#### `-n` — Number of lines

```bash
tail -n 5 file.txt
```

#### `-c` — Number of bytes

```bash
tail -c 50 file.txt
```

* * *

## 4\. Powerful `tail` Features 🔥

### 4.1 Follow file updates (`-f`)

```bash
tail -f logfile.log
```

Used to monitor logs in real time.

* * *

### 4.2 Follow with retry (`-F`)

```bash
tail -F logfile.log
```

Handles log rotation safely.

* * *

### 4.3 Start from a specific line

```bash
tail -n +20 file.txt
```

> Displays file from line 20 onward

* * *

## 5\. `head` vs `tail`

| Feature | head | tail |
| --- | --- | --- |
| Default output | First 10 lines | Last 10 lines |
| View start of file | ✅   | ❌   |
| View end of file | ❌   | ✅   |
| Follow changes | ❌   | ✅   |
| Log monitoring | ❌   | ✅   |

* * *

## 6\. Using with Pipes

### Preview command output

```bash
ps aux | head
```

### Inspect recent activity

```bash
dmesg | tail
```

* * *

## 7\. Real-World Use Cases

### Logs

```bash
tail -n 50 /var/log/syslog
```

### Watch live logs

```bash
tail -f /var/log/auth.log
```

### Check file headers

```bash
head -n 1 data.csv
```

* * *

* * *

* * *

# `echo` Command in Linux

## 1\. Introduction

`echo` is a basic Linux command used to **display text or variables** to the terminal.

- Often used in **scripts, pipelines, and terminal output**
    
- Prints **strings, variables, special characters, or command results**
    

* * *

## 2\. Basic Syntax

```bash
echo [OPTION]... [STRING]...
```

- If no STRING is provided, `echo` prints a **blank line**

* * *

## 3\. Simple Examples

### 3.1 Print text

```bash
echo "Hello, World!"
```

### 3.2 Print a variable

```bash
name="Sushil"
echo "Hello, $name"
```

### 3.3 Print command output

```bash
echo "Today is $(date)"
```

* * *

## 4\. Common Options

| Option | Description |
| --- | --- |
| `-n` | Do not print the trailing newline |
| `-e` | Enable interpretation of backslash escapes |
| `-E` | Disable backslash escapes (default) |

### 4.1 `-n` Example

```bash
echo -n "No newline"
```

### 4.2 `-e` Example (Escapes)

```bash
echo -e "Line1\nLine2\tTabbed"
```

### 4.3 Common Escape Sequences

| Escape | Description |
| --- | --- |
| `\n` | Newline |
| `\t` | Horizontal tab |
| `\r` | Carriage return |
| `\\` | Backslash |
| `\a` | Alert (bell) |
| `\b` | Backspace |

* * *

## 5\. Advanced Usage

### 5.1 Print variables with default values

```bash
echo "${name:-Guest}"
```

### 5.2 Redirect output to file

```bash
echo "Hello" > file.txt
```

Append instead of overwrite:

```bash
echo "Hello again" >> file.txt
```

### 5.3 Using command substitution

```bash
echo "Files: $(ls)"
```

### 5.4 Print colors (ANSI codes)

```bash
echo -e "\e[31mRed Text\e[0m"
```

* * *

## 6\. Common Mistakes

❌ Forgetting quotes

```bash
echo Hello World   # Works, but multiple args treated separately
```

❌ Misusing escape sequences without `-e`

❌ Confusing `>` and `>>`

* * *

## 7\. `echo` in Scripts

### 7.1 Prompt user

```bash
#!/bin/bash

echo -n "Enter your name: "
read name
echo "Hello, $name"
```

### 7.2 Debugging

```bash
echo "Variable x = $x"
```

### 7.3 Logging

```bash
echo "Script started at $(date)" >> script.log
```

* * *

## 8\. Comparison with `printf`

| Feature | echo | printf |
| --- | --- | --- |
| Simple output | ✅   | ✅   |
| Format control | ❌   | ✅   |
| Portability | ✅   | ✅   |
| Escape sequences | Limited | Full |
| No trailing newline | `-n` | `\n` in format |

* * *

* * *

* * *

# `printf` Command in Linux

## 1\. Introduction

`printf` is a **powerful command** in Linux used for **formatted output**.

- More robust and predictable than `echo`
    
- Supports **format specifiers**, padding, alignment, and escape sequences
    
- Commonly used in **scripts, reports, and debugging**
    

* * *

## 2\. Basic Syntax

```bash
printf FORMAT [ARGUMENT]...
```

- `FORMAT` specifies how to display the output
    
- `[ARGUMENT]` provides values to format
    

* * *

## 3\. Basic Examples

### 3.1 Simple string

```bash
printf "Hello World\n"
```

### 3.2 Multiple arguments

```bash
printf "%s %s\n" Hello World
```

> Output: Hello World

### 3.3 Print variable

```bash
name="Sushil"
printf "Hello, %s\n" "$name"
```

* * *

## 4\. Format Specifiers

| Specifier | Description |
| --- | --- |
| `%s` | String |
| `%d` | Integer (decimal) |
| `%f` | Floating point |
| `%x` | Hexadecimal |
| `%o` | Octal |
| `%c` | Character |

### 4.1 Width and Precision

```bash
printf "%10s\n" "Hi"    # Right align, width 10
printf "%-10s\n" "Hi"   # Left align, width 10
printf "%.2f\n" 3.14159 # 2 decimal places
```

* * *

## 5\. Escape Sequences

| Escape | Description |
| --- | --- |
| `\n` | Newline |
| `\t` | Horizontal tab |
| `\\` | Backslash |
| `\a` | Alert (bell) |
| `\r` | Carriage return |

```bash
printf "Line1\nLine2\tTabbed\n"
```

* * *

## 6\. Advanced Usage

### 6.1 Looping with printf

```bash
for i in {1..5}; do
  printf "%d squared = %d\n" "$i" $(($i*$i))
done
```

### 6.2 Table Output

```bash
printf "%-10s %-10s\n" Name Age
printf "%-10s %-10d\n" Alice 25
printf "%-10s %-10d\n" Bob 30
```

### 6.3 Multiple Formats

```bash
printf "%s %d %.2f\n" Alice 25 3.14159
```

### 6.4 Using with Commands

```bash
printf "Disk Usage: %s\n" "$(df -h)"
```

### 6.5 Redirect Output

```bash
printf "Hello World\n" > file.txt
printf "Append Line\n" >> file.txt
```

* * *

## 7\. Comparison with `echo`

| Feature | echo | printf |
| --- | --- | --- |
| Simple output | ✅   | ✅   |
| Format control | ❌   | ✅   |
| Portability | ✅   | ✅   |
| Escape sequences | Limited | Full |
| No trailing newline | `-n` | Explicit in format |

&nbsp;

* * *

* * *

* * *

# `nl` Command in Linux

## 1\. Introduction

`nl` (number lines) is a Linux command used to **number the lines of a file**.

- Useful for **code, logs, and scripts**
    
- Supports **different numbering formats and sections**
    

> Unlike `cat -n`, `nl` provides more control over which lines to number.

* * *

## 2\. Basic Syntax

```bash
nl [OPTION]... [FILE]...
```

- If no FILE is provided, `nl` reads from **standard input**

* * *

## 3\. Basic Usage

### 3.1 Number all lines

```bash
nl file.txt
```

### 3.2 Number only non-empty lines

```bash
nl -b t file.txt
```

> `-b t` = body lines only (skip blank lines)

* * *

## 4\. Common Options

| Option | Description |
| --- | --- |
| `-b STYLE` | Numbering style: `a`\=all, `t`\=non-empty, `n`\=no line numbers |
| `-n FORMAT` | Line number format: `ln`\=left, `rn`\=right, `rz`\=right zero-padded |
| `-w WIDTH` | Width of line numbers (default 6) |
| `-s STRING` | Separator between number and line (default tab) |

### 4.1 Examples

```bash
nl -b a file.txt              # number all lines
nl -b t -n rz -w 4 file.txt   # non-empty lines, zero-padded width 4
nl -s '. ' file.txt            # custom separator
```

* * *

## 5\. Numbering Sections

- `nl` can number **header, body, and footer separately**
    
- `-h`, `-b`, `-f` options control numbering
    

```bash
nl -h a -b t -f a file.txt
```

* * *

## 6\. Using with Pipes

### 6.1 Number output of commands

```bash
ls -l | nl
```

### 6.2 Combine with `grep`

```bash
ls -l | nl | grep pattern
```

* * *

* * *

* * *

# `od` Command in Linux

## 1\. Introduction

`od` (octal dump) is a Linux command used to **display file contents in various formats**, mainly for **binary, non-printable, or raw data**.

- Commonly used for **debugging, inspecting binary files, and data analysis**
    
- Default output is in **octal format**
    

* * *

## 2\. Basic Syntax

```bash
od [OPTION]... [FILE]...
```

- If no FILE is provided, `od` reads from **standard input**
    
- Supports multiple files
    

* * *

## 3\. Basic Usage

### 3.1 View a file in octal (default)

```bash
od file.txt
```

### 3.2 View standard input

```bash
echo "Hello" | od
```

* * *

## 4\. Common Options

| Option | Description |
| --- | --- |
| `-A, --address-radix=RADIX` | Display file offsets in `o` (octal), `d` (decimal), `x` (hex), or `n` (none) |
| `-t, --format=TYPE` | Select output format (e.g., `o`, `x`, `d`, `c`, `f`) |
| `-j, --skip-bytes=BYTES` | Skip bytes at start |
| `-N, --read-bytes=BYTES` | Read only specified bytes |
| `-v` | Show all data (do not suppress repeated lines) |

* * *

## 5\. Output Formats

### 5.1 Octal (default)

```bash
od file.txt
```

### 5.2 Hexadecimal

```bash
od -t x1 file.txt
```

### 5.3 Decimal bytes

```bash
od -t u1 file.txt
```

### 5.4 Characters

```bash
od -c file.txt
```

> Displays printable characters and escape sequences for non-printable ones

### 5.5 Floating point

```bash
od -t f4 binaryfile
```

* * *

## 6\. Advanced Usage

### 6.1 Skip first N bytes

```bash
od -j 5 file.txt
```

### 6.2 Read only N bytes

```bash
od -N 10 file.txt
```

### 6.3 Combine skip and read

```bash
od -j 5 -N 10 file.txt
```

### 6.4 View multiple formats

```bash
od -t x1 -t c file.txt
```

### 6.5 Check file contents with offset in hex

```bash
od -A x -t x1 file.txt
```

* * *

## 7\. Real-World Use Cases

### 7.1 Inspect binary files

```bash
od -t x1 program.bin | less
```

### 7.2 Debug non-printable characters

```bash
echo -e "Hello\nWorld" | od -c
```

### 7.3 Examine network packets or logs

```bash
od -t x1 packet.log
```

### 7.4 Verify file integrity

```bash
od -t x1 file1 | md5sum
od -t x1 file2 | md5sum
```

* * *

* * *

* * *

# `strings` Command in Linux 

## 1\. Introduction

`strings` is a Linux command used to **extract readable text from binary files**.

- Primarily used to **inspect executables, binary data, or non-printable files**
    
- Finds sequences of printable characters
    
- Useful for **debugging, reverse engineering, and forensics**
    

* * *

## 2\. Basic Syntax

```bash
strings [OPTION]... [FILE]...
```

- Reads **binary files** by default
    
- If no FILE is given, reads from **standard input**
    

* * *

## 3\. Basic Usage

### 3.1 Extract strings from binary file

```bash
strings a.out
```

### 3.2 Extract strings from standard input

```bash
cat a.out | strings
```

* * *

## 4\. Common Options

| Option | Description |
| --- | --- |
| `-a`, `--all` | Scan the entire file, not just the data section |
| `-n <number>`, `--bytes=<number>` | Minimum string length (default 4) |
| `-t <format>` | Show the offset: `o`\=octal, `x`\=hex, `d`\=decimal |
| `-e <encoding>` | Encoding: `s`\=single-7-bit, `S`\=single-8-bit, `b`\=16-bit BE, `l`\=16-bit LE |
| `-f` | Print the file name before each string |

### 4.1 Examples

```bash
strings -n 6 a.out          # extract strings ≥ 6 characters
strings -t x a.out          # show hex offsets
strings -e b a.out          # read as 16-bit big endian
strings -f a.out            # show file name before strings
```

* * *

## 5\. Real-World Use Cases

### 5.1 Inspect executables

```bash
strings /bin/ls | grep VERSION
```

### 5.2 Debug files

```bash
strings core.dump | less
```

### 5.3 Extract embedded text from documents

```bash
strings report.pdf | grep Confidential
```

### 5.4 Malware or forensic analysis

```bash
strings suspicious.exe | grep http
```

* * *

## 6\. Combining with Other Commands

### 6.1 With `grep`

```bash
strings a.out | grep printf
```

### 6.2 With `less`

```bash
strings a.out | less
```

### 6.3 With `wc` for counting strings

```bash
strings a.out | wc -l
```

## 7\. Scripts and Automation

```bash
#!/bin/bash

# Extract all strings ≥ 6 chars from multiple binaries
for f in /usr/bin/*; do
  strings -n 6 "$f" | grep -i version
  echo "---"
done
```

* * *

* * *

* * *

# `watch` Command in Linux

## 1\. Introduction

`watch` is a Linux command used to **execute a command repeatedly and display its output** in real-time.

- Ideal for **monitoring changes in files, system stats, or command output**
    
- Default interval is **2 seconds**
    
- Useful for **dynamic observation without writing scripts**
    

* * *

## 2\. Basic Syntax

```bash
watch [OPTION] COMMAND
```

- `COMMAND` = any Linux command you want to run repeatedly
    
- If no interval is specified, default = 2 seconds
    

* * *

## 3\. Basic Usage

### 3.1 Monitor directory changes

```bash
watch ls -l
```

### 3.2 Monitor system processes

```bash
watch ps aux | grep nginx
```

### 3.3 Check disk usage repeatedly

```bash
watch df -h
```

* * *

## 4\. Common Options

| Option | Description |
| --- | --- |
| `-n SECONDS` | Specify update interval in seconds |
| `-d` | Highlight differences between updates |
| `-t` | Turn off header (interval and command info) |
| `-b` | Beep if command returns a non-zero exit |
| `-g` | Exit when the command output changes |

### 4.1 Examples

```bash
watch -n 1 df -h            # update every 1 second
watch -d free -m             # highlight memory changes
watch -t ls                  # no header
watch -b ping -c 1 google.com  # beep on error
```

* * *

## 5\. Real-World Use Cases

### 5.1 Monitor server logs

```bash
watch tail -n 20 /var/log/syslog
```

### 5.2 Monitor CPU usage

```bash
watch -n 1 grep 'cpu ' /proc/stat
```

### 5.3 Watch active network connections

```bash
watch ss -tuln
```

### 5.4 Track file changes

```bash
watch md5sum important_file.txt
```

* * *

## 6\. Combining with Other Commands

### 6.1 With `grep`

```bash
watch "ps aux | grep nginx"
```

> Use quotes to pass piped commands

### 6.2 With `tail`

```bash
watch "tail -n 50 /var/log/syslog"
```

### 6.3 With `du` for disk usage

```bash
watch -n 5 du -sh /home/*
```

* * *

## 7\. Scripts and Automation

```bash
#!/bin/bash
# Monitor disk usage every 10 seconds
watch -n 10 df -h
```

```bash
# Monitor log and highlight differences
watch -d tail -n 50 /var/log/syslog
```

* * *

* * *

* * *

# `view` Command in Linux

## 1\. Introduction

`view` is a Linux command used to **open files in read-only mode** using the **Vim editor**.

- Prevents accidental modifications
    
- Ideal for inspecting **configuration files, logs, and scripts**
    
- `view` is essentially **`vim -R`**
    

> Think of `view` as a safe way to browse files with Vim's power but without editing risks.

* * *

## 2\. Basic Syntax

```bash
view [OPTION]... [FILE]...
```

- Opens specified FILE(s) in **read-only Vim mode**
    
- Supports multiple files
    

* * *

## 3\. Basic Usage

### 3.1 Open a file in read-only mode

```bash
view file.txt
```

### 3.2 Open multiple files

```bash
view file1.txt file2.txt
```

### 3.3 Open with line number

```bash
view +25 file.txt   # start at line 25
```

* * *

## 4\. Navigation in `view`

Since `view` uses Vim:

### 4.1 Basic movement

| Key | Action |
| --- | --- |
| h   | Move left |
| j   | Move down |
| k   | Move up |
| l   | Move right |
| G   | Go to end of file |
| gg  | Go to beginning of file |
| Ctrl+f | Page forward |
| Ctrl+b | Page backward |

### 4.2 Searching

| Key | Action |
| --- | --- |
| /pattern | Search forward |
| ?pattern | Search backward |
| n   | Repeat search forward |
| N   | Repeat search backward |

### 4.3 Quit

```bash
:q
```

> Since file is read-only, no changes are saved

* * *

## 5\. Common Options

| Option | Description |
| --- | --- |
| `+NUM` | Start at line NUM |
| `-R` | Enforce read-only mode (same as default) |
| `-c CMD` | Execute Vim command after opening |
| `-y` | Open in easy mode read-only |

### 5.1 Examples

```bash
view +10 /etc/passwd        # start at line 10
view -c "set number" file.txt  # show line numbers
```

* * *

## 6\. Real-World Use Cases

### 6.1 Inspect config files safely

```bash
view /etc/nginx/nginx.conf
```

### 6.2 Check logs without modifying

```bash
view /var/log/syslog
```

### 6.3 Review scripts

```bash
view my_script.sh
```

* * *

## 7\. Combining with Other Commands

### 7.1 Open last modified file in a directory

```bash
view $(ls -t | head -n 1)
```

### 7.2 Open file from find

```bash
view $(find /etc -name '*.conf' | head -n 1)
```

## 8\. Scripts and Automation

```bash
#!/bin/bash
# Open most recent config file read-only
latest=$(ls -t /etc/*.conf | head -n 1)
view "$latest"
```

* * *

* * *

* * *
