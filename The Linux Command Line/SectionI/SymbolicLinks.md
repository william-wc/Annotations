# Symbolic links

[Prev](ManipulatingFilesAndDirectories.md) | [Next](WorkingWithCommands.md)

`lrwxrwxrwx 1 root root 11 2012-08-11 07:34 libc.so.6 -> libc-2.6.so`

Notice the first letter `l`, denotes a **symbolic link/soft link/symlink**
Used to reference a file by multiple names
`libc.so.6 -> libc-2.6.so` here the name before `->` is the current file name, and after it, is the location/name of the referenced file

## Hard Links

By default, every file has a single hard link that gives the file it's name. When we create a hard link, we create an additional directory entry for a file. But they have two important limitations

- A hard link cannot reference a file outside its own filesystem. This means a link cannot reference a file that is not on the same disk partition as the link itself
- A hard link cannot reference a *directory*

A hard link is indistinguishable from the file itself, unlike a directory list containing asymbolic link, a directory list containing a hard link shows no special indication of the link. When a hard link is deleted, the link is removed but the contents of the file itself continue to exist until **all** links to the file are deleted

## Symbolic Links

Symbolic links were created to overcome the limitations of hard links. Symbolic links work by creating special type of file that contains a text pointer to the preferenced file or directory. In this regard they much as a shortcut.
A file pointed to by a symbolic link and the symbolic link itself are largely indistinguishable from one another. When you write something to the symbolic link, the file referenced will also be written.

- However when you delete a symbolic link, only the link is deleted, the file will continue to exist.
- But when you delete the referenced file, the link will also continue to exist, but it will point to nothing (it is said to be broken)

## Creating Links

The `ln` command is used to create either **hard** or **symbolic** links
`ln file link` - creates a hard link
`ln -s item link` - creates a symbolic link where `item` is either a file or directory

[Prev](ManipulatingFilesAndDirectories.md) | [Next](WorkingWithCommands.md)
