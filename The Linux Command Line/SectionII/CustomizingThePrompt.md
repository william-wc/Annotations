# Customizing the Prompt

[Intro](../Intro.md) | [Prev](GentleIntroductionToVI.md)

## Anatomy of a Promp

The default prompt looks something liek this

`[me@linuxbox ~]$`

It contains the username, hostname and current working directory
The prompt is defined by an environment variable named `PS1 (Prompt String 1)`

```shell
[me@linuxbox ~]$ echo $PS1
[\u@\h \W]\$
```

### Escape Codes used in Shell Prompts

| Sequence | Value Displayed |
|:---------|:----------------|
| `\a` | ASCII bell, this makes the computer beep when it is encountered |
| `\d` | Current date in day, month, date format |
| `\h` | Hostname of the local machine minus the trailing domain name |
| `\H` | Full hostname |
| `\j` | Number of jobs running in the current shell session |
| `\l` | Name of the current terminal device |
| `\n` | A newline character |
| `\r` | A carriage return |
| `\s` | Name of the shell program |
| `\t` | Current time in 24-hour, hours:minutes:seconds format |
| `\T` | Current time in 12-hour format |
| `\@` | Current time in 12-hour, AM/PM format |
| `\A` | Current time in 24-hour, hours:minutes format |
| `\u` | Username of the current user |
| `\v` | Version number of the shell |
| `\V` | Version and release numbers of the shell |
| `\w` | Name of the current working directory |
| `\W` | Last part of the current working directory name |
| `\!` | History number of the current command |
| `\#` | Number of commands entered during this shell session |
| `\$` | This displays `$` character unless you have superuser privileges, displays `#` instead |
| `\[` | This signals the start of a series of one or more non-printing characters. It is used to embed non-printing control characters that manipulate the terminal emulator in some way, sucha s moving the cursor or changing text colors |
| `\]` | This signals the end of a non-printing character sequence |

## Adding Color

Character color is controlled by sending the terminal and *ANSI escape code* embedded in the stream of characters to be displayed

| Sequence      | Text Color |
|:--------------|:-----------|
| `\033[0;30m`  | Black |
| `\033[0;31m`  | Red |
| `\033[0;32m`  | Green |
| `\033[0;33m`  | Brown |
| `\033[0;34m`  | Blue |
| `\033[0;35m`  | Purple |
| `\033[0;36m`  | Cyan |
| `\033[0;37m`  | Light Gray |
| `\033[1;30m`  | Dark Gray |
| `\033[1;31m`  | Light Red |
| `\033[1;32m`  | Light Green |
| `\033[1;33m`  | Yellow |
| `\033[1;34m`  | Light Blue |
| `\033[1;35m`  | Light Purple |
| `\033[1;36m`  | Light Cyan |
| `\033[1;37m`  | White |

| Sequence     | Background Color |
|:-------------|:-----------------|
| `\033[0;40m` | Black |
| `\033[0;41m` | Red |
| `\033[0;42m` | Green |
| `\033[0;43m` | Brown |
| `\033[0;44m` | Blue |
| `\033[0;45m` | Purple |
| `\033[0;46m` | Cyan |
| `\033[0;47m` | Light Gray |

## Moving the Cursor

Escape codes can be used to position the cursor, this is commonly used to provide some kind of information at a different location of the screen each time the prompt is drawn

| Escape | Code Action |
|:-------|:------------|
| `\033[l;cH` | Move the cursor to line l and column c |
| `\033[nA`   | Move the cursor up n lines |
| `\033[nB`   | Move the cursor down n lines |
| `\033[nC`   | Move the cursor forward n characters |
| `\033[nD`   | Move the cursor backward n characters |
| `\033[2J`   | Clear the screen and move the cursor to the upper-left corner (line 0, column 0) |
| `\033[K`    | Clear from the cursor position to the end of the current line. |
| `\033[s`    | Store the current cursor position |
| `\033[u`    | Recall the stored cursor position |

[Intro](../Intro.md) | [Prev](GentleIntroductionToVI.md)