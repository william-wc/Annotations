# Manipulating Files and Directories

[Intro](../Intro.md) | [Prev](LearningTheShell.md) | [Next](SymbolicLinks.md)

`cp`: Copy files and directories
`mv`: Move/rename files and directories
`mkdir`: Create directories
`rm`: Remove files and directories
`ln`: Create hard/symbolic links

## Wildcards

Wildcards (or globbing) allows the selection of filenames based on patterns of characters

| Wildcard  | Matches |
|:----------|:--------|
| *         | Any characters |
| ?         | Any single character |
| \[characters\] | Any character within the set of **characters** |
| \[!characters\] | Any character **not** member of set **characters** |
| \[\[:class:\]\] | Any character that is a member of the specified **class** |

| Character class   | Matches |
|:------------------|:--------|
| \[:alnum:\]       | Any alphanumeric character |
| \[:alpha:\]       | Any alphabetic character |
| \[:digit:\]       | Any numeral |
| \[:lower:\]       | Any lowercase letter |
| \[:upper:\]       | Any uppercase letter |

| Pattern   | Matches |
|:----------|:--------|
| *         | All files |
| g*        | Any file beginning with `g` |
| b*.txt    | Any file beginning with `b` followed by any characters, ending with `.txt` |
| Data???   | Any file beginning with `Data` followed by exactly three characters |
| \[abc\]*  | Any file beginning with either `a`, `b` or `c` |
| BACKUP.\[0-9\] | Any file beginning with `BACKUP.` followed by exactly one number |
| \[\[:upper:\]\]* | Any file beginning with an uppercase letter |
| \[!:digit:\]\]* | Any file not beginning with a numeral |
| *\[\[:lower:\]123\] | Any file ending with a lowercase letter or the numerals 1, 2 or 3 |

It's also possible to specify a character range
`[a-z]`: any lowercase char from `a` to `z`
`[D-G]`: any uppercase char from `D` to `G`

## Creating Directories

`mkdir directory ...`
`mkdir dir1` - creates a directory `dir1`
`mkdir dir1 dir2 dir3` - creates three directories `dir1`, `dir2` and `dir3`

## Copying Files and Directories

It can be used two different ways

`cp item1 item2` - copy the single file or directory `item1` to file or directory `item2`
`cp item... directory` - copy mutiple items (either files or directories) into a directory
`cp item1 item2 dir1` - copy `file1` and `file2` into `dir1` (`dir1` must exists)
`cp dir1/* dir2` - all files in `dir1` are copied into `dir2`

| Option        | Meaning |
|:--------------|:--------|
| a, archive    | Copy the files and directories and all of their attributes (ownerships and permissions) |
| i, interactive | Before overwritting, prompt the user for confirmation (by default overwrites silently) |
| r, recursive  | Recusrively copy directories and their contents |
| u, update     | When copying files from one directory to another, copy only non-existing or newer files |
| v, verbose    | Display informative messages as the copy is performed |

## Move and Rename Files

The `mv` command performs both file moving and renaming

`mv item1 item2` - Move or rename file or directory `item1` to `item2`
`mv item... directory` - Move one or more items into a directory

`mv` shares many options from `cp`

## Remove Files and Directories

The `rm` command is used to remove (delete) files and directories
`rm item...` - removes multiple items

> **Careful with** `rm`: Unix-like operating systems do not have an undelete command

`rm` command shares many options from `mv` and `cp`

| Option    | Meaning |
|:----------|:--------|
| f, force  | Ignore nonexistent files and do not prompt (overrides `--interactive` option) |

[Intro](../Intro.md) | [Prev](LearningTheShell.md) | [Next](SymbolicLinks.md)
