# The Environment

[Intro](../Intro.md) | [Next](GentleIntroductionToVI.md)

The shell maintains a body of information during the session called *environment*
Data stored in the environment is used by programs to determine facts about our configuration

`printenv` - Print part or all of the environment
`set` - Set shell options
`export` - Export environment to subsequently executed programs
`alias` - Create an alias for a command

## What is Stored in the Environment

The shell two basic types of data in the environment:

- **shell variables**: Bits of data placed there by the `bash`
- **environment variables**: Basically everything else

In addition to *variables*, shell also stores some programmatic data: **aliases** and **shell functions**

### Examining the Environment

`printenv` to see a list of environment variables and their values
`printenv USER` to see the `USER` variable value

`set | less` when used without options or arguments, displays both the shell and environment variables as well as shell functions

`echo $HOME` it is also possible to view contents of a single variable

#### Some Interesting Variables

| Variable | Contents |
|:---------|:---------|
| DISPLAY   | Name of your display |
| EDITOR    | The name of the program to be used for text editing |
| SHELL     | The name of your shell program |
| HOME      | The pathname of your home directory |
| LANG      | Defines the character set and collation order of your language |
| OLD_PWD   | The name of the program to be used for pagin output (usually `/usr/bin/less`) |
| PATH      | A colon-separated list of directories that are searched when you enter the name of an executable program |
| PS1       | *Prompt String 1*. This defines the contents of your shell prompt |
| PWD       | The current working directory |
| TERM      | The name of your terminal type |
| TZ        | Specifies your timezone |
| USER      | Your username |

## How is the Environment Established

On login the `bash` program starts and reads a series of configuration scripts called `startup files` which define the default environment shared by all users
The exact sequence dependes on the type of shell session being started

### Login and Non-Login Shells

There are two kinds of shell sessions

- **Login shell session**: We are prompted for our username and password
- **Non-login shell session**

#### Startup Files for Login Shell Sessions

| File | Contents |
|:-----|:---------|
| /etc/profile      | A global configuration script that applies to all users |
| ~/.bash_profile   | A user's personal startup file. Can be used to extend or override settings in the global configuration script |
| ~/.bash_login     | If the above is not found, `bash` attempts to read this script |
| ~/.profile        | If the previous two are not found, `bash` attempts to read this file |

#### Startup Files for Non-Login Shell Sessions

| File | Contents |
|:-----|:---------|
| /etc/bash.bashrc  | A global configuration script that applies to all users |
| ~/.bashrc         | A user's personal startup file. Can be used to extend or override settings in the global configuration script |

In addition to reading the startup files above, *non-login shells* inherit the environment from their parent process usually a login shell

### What's in a Startup File

```bash
# .bash_profile
# Get the aliases and functions
if [ -f ~/.bashrc ]; then
        . ~/.bashrc
fi
# User specific environment and startup programs
PATH=$PATH:$HOME/bin
export PATH
```

Lines that begin with `#` are comments

This is how a login shell gets the contents of `.bashrc`

Shell finds it's commands by searching the `PATH` variable, usually it's set by the `/etc/profile` with `PATH=$PATH:$HOME/bin`

Lastly `export PATH` tells the shell to make the contents of `PATH` available to child processes of this shell

## Modifying the Environment

### Which Files Should We Modify

As a general rule, to add directories to `PATH` or define additional environment variables, place those changes in `.bash_profile` (or equivalent `.profile` file)
For everything else, use `.bashrc`

### Activating the Changes

The changes in `.bashrc` will not take effect until the `.bashrc` file is read:
`source .bashrc`

[Intro](../Intro.md) | [Next](GentleIntroductionToVI.md)
