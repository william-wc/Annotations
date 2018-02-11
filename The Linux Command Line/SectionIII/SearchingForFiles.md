# Searching for Files

`locate` - Find files by name
`find` - Search for files in a directory hierarchy
`xargs` - Build and execute command lines from standard input
`touch` - Change file times
`stat` - Display file or filesystem status

## `locate` - Find Files the Easy Way

The `locate` program performs a rapid database search of pathnames and then outputs every name that matches a given substring

```shell
$ locate bin/zip
/usr/bin/zip
/usr/bin/zipcloak
/usr/bin/zipgrep
/usr/bin/zipinfo
/usr/bin/zipnote
/usr/bin/zipsplit
```

The `locate` database is created by another program named `updatedb`. Usually it is run periodically as a `cron job`

## `find` - Find Files the Hard Way

The `find` program searches a given directory (and its subdirectories) based on a variety of attributes
It can be used to identify files that meet specific criteria, through the application of *tests*, *actions* and *options*

### Tests

List of directories
`find ~ -type d | wc -l`

#### `find` File Types

| File Type | Description |
|:----------|:------------|
| b | Block special device file |
| c | Character special device file |
| d | Directory |
| f | Regular file |
| l | Symbolic link |

It's also possible to search by file size or filename by adding additional *tests*

`find ~ -type f -name "*.jpg" -size +1M`
Here `-name` followed by the wildcard pattern enclosed in quotes to prevent pathname expansion, also `-size` test followed by string `+1M` indicating files larger than `1 megabyte`

#### `find` Size Units

| Character | Unit |
|:----------|:-----|
| b | 512-byte blocks (default unit if unspecified) |
| c | Bytes |
| w | 2-byte words |
| k | Kilobytes (1024 bytes) |
| M | Megabytes (1,048,576 bytes) |
| G | Gigabytes (1,073,741,824 bytes) |

#### `find` Tests

| Test | Description |
|:-----|:------------|
| -cmin n | Match files or directories whose content or attributes were last modified exactly `n` minutes ago (use `+-` to specify fewer or greater) |
| -cnewer file | Match files or directories whose contents or attributes were last modified more recently than those of `file` |
| -ctime n | Match files or directories whose contents of attributes were last modified `n*24` hours ago |
| -empty | Match empty files and directories |
| -group name | Match file or directories belonging to group `name`, expressed as name or numeric group ID |
| -iname pattern | Like `-name` but case **insensitive** |
| -inum n | Match files with inode number `n`. Useful for finding hard links |
| -mmin n | Match files or directories whose contents were modified `n` minutes ago |
| -mtime n | Match files or directories whose contents only were last modified `n*24` hours ago |
| -name pattern | Match files and directories with the specified wildcard `pattern` |
| -newer file | Match files and directories whose contents were modified more recently than the specified `file` |
| -nouser | Match file and directories that do not belong to a valid user |
| -nogroup | Same as above but with groups |
| -perm mode | Match files or directories that have permissions set to the specified `mode` (octal or symbolic notation) |
| -samefile name | Similar to `-inum`, same inode number as file `name` |
| -size n | Match files of size `n` |
| -type c | Match files of type `c` |
| -user name | Match files or directories belonging to `name` (expressed by username or numeric user ID) |

## Operators

Used to describe **logical relationships** between the tests

`find ~\( -type f -not -perm 0600 \) -or \( -type d -not -perm 0700 \)`
Look for all the files with permissions that are not `0600` and the directories with permissions that are not `0700`

### `find` Logical Operators

| Operator | Description |
|:---------|:------------|
| -and, -a  | Match if the tests on both sides of the operator are **true** (default operator if none are present) |
| -or, -o   | Match if either side of the operator is **true** |
| -not, -!  | Match if the test following the operator is **false** |
| ()        | Group tests and operators together to form alrger expressions |

`expr1 -operator expr2`

## Actions

`find` allows actions to be performed bbased on the search results

### Predefined actions

There are a set of predefined actions and several ways to apply user-defined actions

| Action    | Description |
|:----------|:------------|
| -delete   | Delete the currently matching file |
| -ls       | Perform the equivalent of `ls -dils` on the matching file. Output is sent to stdout |
| -print    | Output the full pathname of the matching file to stdout (default is no other is specified) |
| -quit     | Quit once a match has been made |

`find ~ -type f -name '*.BAK' -delete`
same as
`find ~ -type f -and -name '*.BAK' -delete`
Deletes all files in the user's home directory with ending `.BAK`

### User-Defined Actions

Invoke arbitrary commands with `-exec`

`-exec command {} ;`
`{}` is a symbolic representation of the current pathname
The semicolon is a required delimiter indicating the end of the command

`-delete` = `-exec rm '{}' ';'`

`find ~ -type f -name 'foo*' -ok ls -l '{}' ';'`
Search for files with names starting with `foo` and execute command `ls -l` each time one is found, the `-ok` action prompts the user before the `ls` command is executed

#### Improving Efficiency

When `-exec` action is used, it launches a new instance of the specified command each time a matching file is found
To group them to execute the command only once there are two different ways:

`find ~ -type f -name 'foo*' -exec ls -l '{}' +`
or
`find ~ -type f -name 'foo*' -print | xargs ls -l`

## Options

Options are used to control the scope of a `find` search. They may be included with other `tests` and `actions`

| Option | Description |
|:-------|:------------|
| -depth | Direct `find` to process a directory's files before the directory itself |
| -maxdepth levels | Set the maximum number of levels that `find` will descend into a directory tree |
| -mindepth levels | Set the minimum number of levels that `find` will descend into a directory tree |
| -mount | Direct `find` to traverse directories that are mounted on other filesystems |
