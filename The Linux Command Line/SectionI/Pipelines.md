# Pipelines

[Prev](Redirection.md) | [Next]()

The ability of commands to read data from standard input and send to standard output
Uses the pipe operator `|`, the standard output of one command can be `piped` into the standard input of another

> `command1 | command2`

`ls -l /usr/bin | less`

This was it's possible to conveniently examine the output of any command the produces standard output

## Filters

Pipelines are often used to perform complex operations on data, it's possible to put several commands together into a *pipeline*
The commands used this way are referred to as *filters*, they take input, change it somehow and then output it

`ls /bin /usr/bin | sort | less`

Sorts a list of all executable programs in `/bin` and `/usr/bin` then view the list in `less`

### `uniq` - Report or Omit Repeated Lines

`ls /bin /usr/bin | sort | uniq | less`
Removes any duplicates from the output of sort command

`ls /bin /usr/bin | sort | uniq -d | less`
Shows only duplicates instead

### `wc` - Print Line, Word and Byte Counts

The `wc` (Word Count) command is used to display the number of lines words and bytes contained in files

`wc ls-output.txt`
`7902  64566  503634 ls-output.txt` <- Lines, Words and Bytes respectively

`ls /bin /usr/bin | sort | uniq | wc -l` -> `2728`
`-l` option returns only the number of lines

### `grep` - Print Lines Matching a Pattern

`grep` is a powerful program used to find text patterns within files

> `grep pattern [file...]`

When `grep` encounters a pattern in the file, it prints out the lines containing it

```bash
$ ls /bin /usr/bin | sort | uniq | grep zip
bunzip2
bzip2
bzip2recover
funzip
gunzip
gzip
unzip
unzipsfx
zip
zipcloak
zipdetails
zipdetails5.16
zipdetails5.18
zipgrep
zipinfo
zipnote
zipsplit
```

### `head/tail` - Print First/Last Part of Files

`head` prints the first 10 lines of a file, same with `tail`
`head -n 5` prints the first 5 lines of a file

### `tee` - Read from Stdin or Output to Stdout and Files

`tee` creates a `T` fitting on the pipe
It reads standard input and copies it to both standard output (allowing data to continue down the pipeline) and to one or more files
This allows capturing a pipeline's contents at an intermediate stage of processing

`ls /usr/bin | tee ls.txt | grep zip`
Lists all in `/usr/bin`, sends it to `ls.txt` and also sends it to `grep zip`

[Prev](Redirection.md) | [Next]()
