* * *

**Sorting/filtering:** `sort` `uniq` `wc`

* * *

* * *

# `sort` Command in Linux

`sort` is a Linux command used to **sort lines of text files or standard input**. It supports sorting by alphabetical order, numeric values, months, versions, and custom fields, making it essential for text and data processing.

* * *

## 1\. What is `sort`

`sort` arranges lines of text in a specific order.

Typical use cases:

- Sorting CSV or TSV files
    
- Ordering log files
    
- Preparing data for `uniq` or `join`
    

* * *

## 2\. How `sort` Works

```
input → comparison rules → ordered output
```

Key points:

- Sorts line by line
    
- Default sort is lexicographical
    
- Output goes to standard output unless redirected
    

* * *

## 3\. Basic Syntax

```bash
sort [OPTIONS] [FILE...]
```

Example:

```bash
sort file.txt
```

* * *

## 4\. Alphabetical Sorting

```bash
sort names.txt
```

Descending order:

```bash
sort -r names.txt
```

* * *

## 5\. Numeric Sorting (`-n`)

```bash
sort -n numbers.txt
```

Descending numeric:

```bash
sort -nr numbers.txt
```

* * *

## 6\. Reverse Sorting (`-r`)

```bash
sort -r file.txt
```

* * *

## 7\. Sorting by Fields (`-k`)

Sort by second field:

```bash
sort -k 2 file.txt
```

Sort by second field numerically:

```bash
sort -k 2,2n file.txt
```

* * *

## 8\. Field Delimiters (`-t`)

Default delimiter is space.

Custom delimiter:

```bash
sort -t ',' -k 2 data.csv
```

* * *

## 9\. Unique Sorting (`-u`)

Remove duplicates while sorting:

```bash
sort -u file.txt
```

Equivalent to:

```bash
sort file.txt | uniq
```

* * *

## 10\. Month Sorting (`-M`)

Sort by month names:

```bash
sort -M months.txt
```

* * *

## 11\. Version Sorting (`-V`)

Sort version numbers correctly:

```bash
sort -V versions.txt
```

* * *

## 12\. Human Numeric Sort (`-h`)

Sort human-readable sizes:

```bash
sort -h sizes.txt
```

Works with:

```
10K
2M
1G
```

* * *

## 13\. Case-Insensitive Sorting (`-f`)

```bash
sort -f names.txt
```

* * *

## 14\. Sorting Files In-Place (`-o`)

```bash
sort -o file.txt file.txt
```

Safely replaces original file.

* * *

## 15\. Using `sort` in Pipelines

With `cut`:

```bash
cut -d ',' -f 2 data.csv | sort
```

With `uniq`:

```bash
sort file.txt | uniq -c
```

With `join`:

```bash
join <(sort f1.txt) <(sort f2.txt)
```

* * *

## 16\. Common Use Cases

Sort log by status code:

```bash
awk '{ print $9 }' access.log | sort -n
```

Find top values:

```bash
sort -nr scores.txt | head
```

Prepare file for join:

```bash
sort -t ',' -k 1 data.csv
```

* * *

## 17\. Limitations of `sort`

- No content transformation
    
- Requires memory for large files
    
- Sorting rules can be complex
    

For complex logic, use `awk`.

* * *

## 18\. Tips and Best Practices

- Always sort before `uniq` or `join`
    
- Use `-n` for numbers
    
- Use `-k` with field ranges
    
- Combine with `head` or `tail`
    

* * *

## 19\. Summary Cheat Sheet

```bash
sort file
sort -n file
sort -r file
sort -u file
sort -t ',' -k 2 file
sort -nr file
sort -V file
sort -h file
```

* * *

## Final Notes

`sort` is a foundational command in Linux text processing. It integrates seamlessly with `uniq`, `cut`, `join`, `awk`, and `sed` to build powerful data-processing pipelines.

* * *

* * *

# `uniq` Command in Linux

`uniq` is a Linux command used to **report or filter out repeated lines in a file**. It is commonly paired with `sort` to process structured text and logs efficiently.

* * *

## 1\. What is `uniq`

`uniq` filters or reports **adjacent duplicate lines** in a file.

Typical use cases:

- Counting unique log entries
    
- Removing duplicate lines
    
- Summarizing sorted datasets
    

* * *

## 2\. How `uniq` Works

```
input (lines) → check for duplicates → output
```

Key points:

- Only removes **adjacent duplicates**
    
- Works line by line
    
- Commonly used with `sort`
    

* * *

## 3\. Basic Syntax

```bash
uniq [OPTIONS] [INPUT [OUTPUT]]
```

Example:

```bash
uniq file.txt
```

* * *

## 4\. Removing Duplicate Lines

Remove adjacent duplicates:

```bash
uniq file.txt
```

Remove and save to a new file:

```bash
uniq file.txt unique.txt
```

* * *

## 5\. Counting Repeated Lines (`-c`)

Count occurrences of each line:

```bash
uniq -c file.txt
```

Example output:

```
3 apple
1 banana
2 orange
```

* * *

## 6\. Display Only Duplicates (`-d`)

Show only lines that **repeat**:

```bash
uniq -d file.txt
```

* * *

## 7\. Display Only Unique Lines (`-u`)

Show only lines that **appear once**:

```bash
uniq -u file.txt
```

* * *

## 8\. Ignoring Case (`-i`)

Make comparisons **case-insensitive**:

```bash
uniq -i file.txt
```

* * *

## 9\. Skipping Fields (`-f`) and Characters (`-s`)

Skip fields:

```bash
uniq -f 1 file.txt
```

Skip characters:

```bash
uniq -s 3 file.txt
```

Combine options for advanced filtering:

```bash
uniq -i -f 1 -s 3 file.txt
```

* * *

## 10\. Using `uniq` in Pipelines

With `sort`:

```bash
sort file.txt | uniq
```

Count occurrences:

```bash
sort file.txt | uniq -c
```

Filter only duplicates:

```bash
sort file.txt | uniq -d
```

* * *

## 11\. Combining with `sort`

Since `uniq` only removes adjacent duplicates, always sort first:

```bash
sort file.txt | uniq
```

* * *

## 12\. Common Use Cases

Count unique IPs in log:

```bash
awk '{print $1}' access.log | sort | uniq -c
```

Remove duplicate lines:

```bash
sort file.txt | uniq > clean.txt
```

Show only duplicates:

```bash
sort file.txt | uniq -d
```

* * *

## 13\. Limitations of `uniq`

- Only works on **adjacent duplicates**
    
- Cannot sort by itself
    
- No regex support
    
- Works line by line
    

* * *

## 14\. Tips and Best Practices

- Always use with `sort` for complete duplicate removal
    
- Combine `-c`, `-d`, `-u` for summaries
    
- Use `-i`, `-f`, `-s` for complex datasets
    
- Redirect output to save results
    

* * *

## 15\. Summary Cheat Sheet

```bash
uniq file.txt
uniq -c file.txt
uniq -d file.txt
uniq -u file.txt
sort file.txt | uniq
sort file.txt | uniq -c
```

* * *

## Final Notes

`uniq` is simple but powerful for filtering duplicates and counting occurrences. It complements `sort`, `cut`, `awk`, and `sed` in Linux text-processing pipelines.

* * *

* * *

# `wc` Command in Linux

`wc` (word count) is a Linux command used to **count lines, words, characters, and bytes in files or standard input**. It is useful for quick file statistics and text analysis.

* * *

## 1\. What is `wc`

`wc` provides **summary statistics** of text files or input streams.

Typical use cases:

- Counting lines in logs
    
- Checking word counts in documents
    
- Getting file sizes in bytes or characters
    

* * *

## 2\. How `wc` Works

```
input → counting → output summary
```

Key points:

- Works on files or standard input
    
- Counts based on lines, words, characters, or bytes
    
- Output is printed in a simple column format
    

* * *

## 3\. Basic Syntax

```bash
wc [OPTIONS] [FILE...]
```

Example:

```bash
wc file.txt
```

Output:

```
10 50 300 file.txt
```

- `10` lines, `50` words, `300` bytes

* * *

## 4\. Counting Lines (`-l`)

```bash
wc -l file.txt
```

Output:

```
10 file.txt
```

Counts only the number of lines.

* * *

## 5\. Counting Words (`-w`)

```bash
wc -w file.txt
```

Counts words separated by whitespace.

* * *

## 6\. Counting Characters (`-m`)

```bash
wc -m file.txt
```

Counts characters, including multi-byte characters.

* * *

## 7\. Counting Bytes (`-c`)

```bash
wc -c file.txt
```

Counts raw bytes in the file.

* * *

## 8\. Counting Longest Line Length (`-L`)

```bash
wc -L file.txt
```

Prints the length of the **longest line**.

* * *

## 9\. Using `wc` with Multiple Files

```bash
wc file1.txt file2.txt
```

Output:

```
 10 50 300 file1.txt
 20 100 600 file2.txt
 30 150 900 total
```

* * *

## 10\. Using `wc` in Pipelines

Count lines from `grep`:

```bash
grep error logfile | wc -l
```

Count words from command output:

```bash
ls | wc -w
```

Count characters of a string:

```bash
echo "hello world" | wc -m
```

* * *

## 11\. Common Use Cases

Count lines in a file:

```bash
wc -l access.log
```

Count words in multiple files:

```bash
wc -w *.txt
```

Find longest line:

```bash
wc -L file.txt
```

* * *

## 12\. Limitations of `wc`

- Cannot count specific words or patterns
    
- Limited to overall counts, no context
    
- For advanced text analysis, use `awk` or `grep`
    

* * *

## 13\. Tips and Best Practices

- Combine `wc` with `grep` for pattern-based counting
    
- Use `wc -m` for character counts with multibyte support
    
- Always sort or filter before counting for precise analysis
    
- Redirect output to files when needed
    

* * *

## 14\. Summary Cheat Sheet

```bash
wc file.txt           # lines, words, bytes
wc -l file.txt        # lines only
wc -w file.txt        # words only
wc -m file.txt        # characters only
wc -c file.txt        # bytes only
wc -L file.txt        # longest line
grep pattern file | wc -l  # count occurrences
```

* * *

## Final Notes

`wc` is a lightweight and versatile tool for quick text statistics. It complements `grep`, `cut`, `awk`, `sort`, and `uniq` in Linux text-processing workflows.
