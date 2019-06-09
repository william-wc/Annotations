# Basics

Page movement: `SPC n [, . < >]`
`C-v` move forward one screenful
`M-v` move backward one screenful
`C-d` move forward half screen
`C-u` move backward half screen

`C-l` clear screen and redisplay all text where cursor is in the middle (can be toggled for other modes)

List all keybindings: `C-h b`
Find keybinding definition and declaration: `C-h k`

## Basic Cursor Control

Keep your hands in the standard position and use the commands:
`C-p` | `M-x previous-line` Move forward a char
`C-b` | `M-x backward-char` Move backward a char

`M-f` | `e` Move forward a word
`M-b` | `` Move backward a word

`C-n` | `M-x next-line` Move to next line
`C-p` | `` Move to previous line

`C-a` Move to beginning of line
`C-e` | `M-x move-end-of-line` Move to end of line

`M-a` Move back to beginning of sentence
`M-e` Move forward to end of sentence

`M-<` / `M->` Move to the beginning/end of the buffer

`C-u` repeat command
- `C-u 8 *` repeats '*' 8 times

## Inserting and Deleting

`M-<DEL>` Kill the word immediately before the cursor
`M-d` Kill the word immediately after the cursor

`C-k` Kill from the cursor position to the **end of line**
`M-k` Kill to the end of the current **sentence**

> **Killing** text can be reinserted (at any position) whereas **deleted*** text cannot

Yanking reinserts the last killed text `C-y` | `s-v`
When doing several `C-k`'s in a row, all of the killed text is saved together, so that one `C-y` will yank all of the lines at once.

## Undo

To simply undo changes with `C-/` or `u`

## Files and Buffers

`C-x C-f` Find a file
`C-x C-s` Save the file
`C-x C-b` List buffers
`C-x b` | `SPC b b` Switch buffer
`C-x s` Same some buffers
`C-x 1` Delete all but one window

## Extending the Command Set

`C-x` character eXtend (followed by one character)
`M-x` Named command eXtend (followed by a long name)

## Auto Save

When you have made changes to a file but didn't explicitly save them yet, emacs periodically creates
Another file surrounded by `#` (e.g.: `hello.c` -> `#hello.c#`)

## Echo Area

When emcas sees that you're typing multicharacter commands slowly, it shows them to you at the bottom
of the screen

## Searching

`C-s` initiates forward search
`C-r` | `` initiates reverse search

Steps:
1) Start with `C-s`
2) Type the string you want to search, any matches will start highlighted
3) Type `C-s` again to search for the next occurance or `<DEL>` to search the previous one
4) Type `<RET>` to terminate the search

## Getting More Help

`C-h ?` Emacs will tell what kinds of help it can give
`C-h c` Displays a very brief description of the command
`C-h k` Displays the documentation of the function
`C-x C-c` Exits Emacs.
