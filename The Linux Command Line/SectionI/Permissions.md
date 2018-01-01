# Permissions

[Prev](AdvancedKeyboardTricks.md) | [Next]()

`id` - Display user identity
`chmod` - Change a file's mode
`umask` - Set the default file permissions
`su` - Run a shell as another user
`sudo` - Execute a command as another user
`chown` - Change a file's owner
`chgrp` - Change a file's group ownership
`passwd` - Change a user's password

## Owners, Group Memebers and Everybody Else

In the Unix security model, a user may `own` files and directories
When a user owns a file or directory, the user has control over it's access
In addition to granting access to a group, an owner may also grant some set of access rights to everybody

## Reading, Writing and Executing

Access rights to files and directories are defined in terms of read, write and execution access

`ls -l foo.txt` -> `-rw-rw-r-- 1 me me ...`

The first character is the *file type*
The next nine characters are the *file mode*, represent the read, write and execute permissions for:

- file's owner
- file's group owner
- everybody else

File Types

| Attribute | File Type |
|:----------|:----------|
| -         | Regular file |
| d         | Directory |
| l         | Symbolic link - All symbolic links have dummy permissions `rwxrwxrwx` |
| c         | *Character special file*, refers to a device that handles data as a stream of bytes |
| b         | *Block special file*, refers to a device that handles data in blocks (hard drive or CD-ROM drive) |

Permission Attributes

| Attribute | Files | Directories |
|:----------|:------|:------------|
| r | Allows a file to be opened and read | Allows a directory's contents to be listed if execute is also set |
| w | Allows a file to be written to (but not renamed or deleted) | Allows files within a directory to be created, deleted and renamed if execute is also set |
| x | Allows a file to be treated as a program and executed | Allows a directory to be entered |

`-rwx------` Regular file that is readable, writable and executable by **file's owner only**
`-rw-------` Regular file that is readable, writable by file owner only
`-rw-r--r--` Regular file that is readable and writable by file owner, only readable by everyone else
`-rwxr-xr-x` Regular file that is readable, writable and executable by owner. Readable and executable by everyone else
`-rw-rw----` Regular file that is readable and writable by owner and group only
`lrwxrwxrwx` Symbolic link, all symbolic links have dummy permissions
`drwxrwx---` Directory owner and group can enter, create, rename and remove files
`drwxr-x---` Directory owner can create, rename and remove files. Group can enter but not alter any of it's contents

## `chmod` - Change File Mode

Supports two distinct ways of specifying mode changes

**Octal** Representation

| Octal | Binary | File Mode |
|:------|:-------|:----------|
| 0     | 000   | --- |
| 1     | 001   | --x |
| 2     | 010   | -w- |
| 3     | 011   | -wx |
| 4     | 100   | r-- |
| 5     | 101   | r-x |
| 6     | 110   | rw- |
| 7     | 111   | rwx |

`-rw-rw-r--` -> `chmod 500 file` -> `-rw-------`

**Symbolic** Representation is divided in three parts, using a combination of characters `u`, `g`, `o` and `a`:

- **whom** the change will affect
- **which** operation will be performedk
- **which** permission will be set

| Symbol | Meaning |
|:-------|:--------|
| u | Means the file or directory **owner** |
| g | **Group** owner |
| o | Short for **others** but means world |
| a | Short for **all**, combination of `u`, `g` and `o` |

If no character is specified, then **all** is assumed

`u+x` - Add execute permission for the owner
`u-x` - Remove execute permission for the owner
`+x` - Add execute permission for all (equivalent to `a+x`)
`o-rw` - Remove read and write permissions from anyone besides the owner and group
`go=rw` - Set the group owner and anyone besides the owner to have read and write permission, if either group or world had execute permissions, remove them
`u+x,go=rx` - Add execute permission for the owner and set permissions for the group and others to read and execute. Multiple specifications may be separated by commas

## `umask` - Set Default Permissions

Sets default permissions given to a file when it is created, uses octal notation to express a `mask` of bits to be removed from a file's mode attributes

`umask` -> `002` or `022`
`> foo.txt` -> `-rw-rw-r--`

## `chown` - Change File Owner and Group

> `chown [owner][:[group]] file...`

`bob` - Changes the ownership of the file from it's current owner to user `bob`
`bob:users` - Changes the ownership of the file from its current owner to user `bob` and changes the file gorup owner to group `users`
`:admins` - Changes the group owner to the group `admins`. File owner is unchanged
`bob:` - Change file owner from current user to user `bob` and changes the group owner to the login group of user `bob`

[Prev](AdvancedKeyboardTricks.md) | [Next]()
