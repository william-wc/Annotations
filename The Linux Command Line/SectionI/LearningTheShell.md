# I - Learning the Shell

[Intro](../Intro.md) | [Prev](../Intro.md) | [Next](ManipulatingFilesAndDirectories.md)

Exploration of the basic language of the command line, including such things as the structure of commands, filesystem navigation, command line editing, and finding help and documentation for commands

## Commands

Commands are often followed by one or more *options* that modify their behavior and, further, by one or more *arguments*, the items upon which the command acts:

> `command -options arguments`

Many commands support *long options*, that consist of a word precede by two dashes `-l` = `--list`
Or multiple short options strung together `-lt`

## Directories

`ls` lists directory contents

| Option | Long Option | Description |
|:-------|:------------|:------------|
| a      | all         | List all files (including hidden files) |
| d      | directory   | Use this option in conjunction with others about the target directory |
| F      | classify    | Appends an indicator character to the end of each listed name (directories end with slash) |
| h      | human-readable | Display file sizes in human-readable format rather than in bytes |
| l      |             | Display results in long format |
| r      | reverse     | Display results in reverse order (usually they are alphabetical) |
| S      |             | Sort results by file size |
| t      |             | Sort by modification time |

## Long listing fields

`-rw-r--r-- 1 root root  423141 2012-04-03  11:05 file.png`

1. `-rw-r--r--`
    Access rights to the file in order of characters:
    1. Type:
        - `-`: regular file
        - `d`: directory
    2. The next three indicate access rights for the file's owner
    3. The next three are for members of the file's group
    4. The last three are for everyone else
2. `1`: File's number of hard links
3. `root`: The user name of the file's owner
4. `root`: The name of the gorup that owns the file
5. `423141`: Size of the file in bytes
6. `2012-04-03 11:05`: Date and time of last modification
7. `file.png`: Name of the file

`file`: Determins the file type

In Unix-like operating systems **everything is a file**

`less`: Examins contents of a file

| Command       | Action |
|:--------------|:-------|
| PAGE UP or b  | Scroll back one page |
| PAGE DOWN or SPACEBAR | Scroll forward one page |
| UP            | Scroll up one line |
| DOWN          | Scroll down one line |
| G             | Move ot the end of the text file |
| 1G or g       | Move to the beginning of the text file |
| /characters   | Search forward to the next occurence of `characters` |
| n             | Search for the next occurence of the previous search |
| h             | Display help screen |
| q             | Quit `less` |

### Directories Found in Linux Systems

| Directory | Comments |
|:----------|:---------|
| /         | The root directory |
| /bin      | Contains binaries (programs) that must be present for the system to boot and run |
| /boot     | Contains the kernel, initial RAM disk image (for drivers needed at boot time) and the boot loader |
| /dev      | Special directory that contains device nodes |
| /etc      | Contains all of the sistem-wde configuration files, and a collection of shell scripts that start each of the system services at boot time |
| /lib      | Contains shared library files used by the core system programs |
| /media    | Contains the mount points for removable media |
| /mnt      | Save as above (older linux systems) |
| /opt      | Is used to install *optional* software |
| /proc     | Not a real filesystem of files on your hard drive, it is a virtual filesystem maintained by the linux kernel |
| /root     | Home directory for the root account |
| /sbin     | System binaries, programs that perform vital system tasks that are generally reserved for the superuser |
| /usr      | Contains all programs and support files used by regular users |
| /usr/bin  | Executable programs installed |
| /usr/lib  | Shared libraries for the programs in /usr/bin |
| /usr/local | Programs that are not included with your distribution but are intended for system-wide use are installed |
| /usr/sbin | Contains more system administration programs |
| /usr/share | Contains all the shared data used by programs in /usr/bin (default configuration files) |
| /var      | Where data that is likely to change is stored, databases, spool files, user mail, etc |

[Intro](../Intro.md) | [Prev](../Intro.md) | [Next](ManipulatingFilesAndDirectories.md)
