# Advanced Keyboard Tricks

[Prev](SeeingTheWorldAsTheShellSeesIt.md) | [Next]()

## Cursor Movement

| Key | Action |
|:----|:-------|
| CTRL-A | Move cursor to the beginning of the line |
| CTRL-E | Move cursor to the end of the line |
| CTRL-F | Move cursor forward one character |
| CTRL-B | Move cursor backward one character |
| ALT-F | Move cursor forward one word |
| ALT-B | Move cursor backward one word |
| CTRL-L | Clean screen and move the cursor to the top left corner |

## Text Editing

| Key | Action |
|:----|:-------|
| CTRL-D | Delete the character at cursor location |
| CTRL-T | Transpose the character at cursor location with the one preceding it |
| ALT-T | Transpose the word at the cursor location with the one preceding it |
| ALT-L | Convert the characters from the cursor location to the end of word to lowercase |
| ALT-U | Convert the characters from the cursorlocation to the end of the word to uppercase |

## Cut and Paste

| Key | Action |
|:----|:-------|
| CTRL-L | Kill text from cursor location tot eh end of line |
| CTRL-U | Kill text from cursor location to the beginning of the line |
| ALT-D | Kill text from cursor location to the end of the current word |
| ALT-Backspace | Kill text from cursor location to the beginning of the current word |
| CTRL-Y | Yank text from the kill-ring and insert at cursor location |

## Searching History

`history | less`
`history | grep /usr/bin`

In case the command line is found at line 88

`!88` -> Bash will expand into the contents of the 88th line in the history list

Another way of searching is by using reverse-index-search (CTRL-R)

### History Expansion

| Sequence  | Action |
|:----------|:-------|
| !!        | Repeat the last command |
| !number   | Repeat history list item number |
| !string   | Repeat last history list item starting with *string* |
| !?string  | Repeat last history list item containing *string* |

[Prev](SeeingTheWorldAsTheShellSeesIt.md) | [Next]()