# Working with Commands

[Prev](SymbolicLinks.md) | [Next]()

What exactly are **commands**:

- **An executable program**:
    Like all those files in `/usr/bin`
    Within this category programs can be *compiled binaries* or programs written in *scripting languages*
- **A command built into the shell itself**:
    `bash` supports a number of commands internally called `shell builtins` (ex: `cd`)
- **A shell function**:
    `Shell functions` are miniature shell scripts incorporated into the `environment`
- **An alias**:
    A command that we can define ourselves, built from other commands

| Command   | Description |
|:----------|:------------|
| type      | Indicate how a command name is interpreted |
| which     | Display which executable program will be executed |
| man       | Display a command's manual page |
| apropos   | Display a list of appropriate commands |
| info      | Display a command's info entry |
| whatis    | Display a very brief description of a command |
| alias     | Create an alias for a command |

## Identifying commands

### `type` - Display a Command's Type

Is a shell builtin that displays the kind of command the shell will execute

`type command`
`type type` - `type is a shell builtin`
`type cp` - `cp is /bin/cp`

### `which` - Display an Executable's Location

Sometimes more than one version of an executable program is installed on a system
`which` determines the exact location of a given executable

`which ls` - `/bin/ls`

`which` works only for executable programs, not builtins or aliases

### `help` - Get Help for Shell Builtins

`help cd` - Description of `cd` command

### `help` - Display Usage Information

Many executable programs uspport a `--help` option that displays a description of the command's supported syntax and options

`mkdir --help`

### `man` - Diplay a Program's Manual Page

Most executable programs intended for command-line use provide a formal piece of documentation called a `manual` or `man page`

`man program`
`man ls` - Displays manual of `ls` command

Man Page Organization
| Section   | Contents |
|:----------|:---------|
| 1         | User commands |
| 2         | Programming interfaces for kernel system calls |
| 3         | Programming interfaces to the C library |
| 4         | Special files such as device nodes and drivers |
| 5         | File formats |
| 6         | Games and amusements such as screensavers |
| 7         | Miscellaneous |
| 8         | System administration commands |

`man section search_term`
`man 5 passwd` - Display the man page describing the file format of the `/etc/passwd` file

### `apropos` - Display Appropriate Commands

It is also possible to search the list of man pages for possible matches based on a search term

### `whatis` - Diplay a Very Brief Description of a Command

Displays the name and a one-line description of a man page matching a specifified keyword

`whatis ls` - `list directory contents`

### `info` - Display a Program's Info Entry

The GNU Project provides an alternative to man pages called *info pages*
The `info` program reads tree-structured `info files`

---

## Creating Your Own Commands with `alias`

It's possible to combine multiple commands by simply chaining them with `;` between them

`alias name='string'` - Creates a new alias `name` that calls `string`
`unalias name` - Removes alias `name`
`alias` - Lists all aliases

Aliases are **non-persistent**, which means they are gone once the **shell session** ends

> Note:
> `;` will run the next command independently if the previous one failed
> `&&` runs the next command **only** if the last one succeeded

`alias foo='cd /usr; ls; cd -'`
The alias `foo` will:

1. Change to `/usr` directory
2. List directory contents with `ls`
3. Go back to where it was before with `cd -`

[Prev](SymbolicLinks.md) | [Next]()
