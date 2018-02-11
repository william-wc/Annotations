# Writing Shell Scripts

## How to Write a Shell Script

1. Write a script
2. Make the script executable
3. Put the script somewhere the shell can find it

### Script file format

```shell
#!/bin/bash
# This is our first script.
echo 'Hello World!'
```

### Good Locations for Scripts

`~/bin` for personal use
`/usr/local/bin` for everyone on a system
`/usr/local/sbin` for the system administrator
Usually, locally supplied software, whether scripts or compiled programs should be placed in `/usr/local` hierarchy, and not `/bin` or `/usr/bin`

---

## Flow Control - Branching with `if`

```shell
if [ $x = 5 ]; then
    echo "x is 5"
else
    echo "x is not 5"
fi
```

```shell
if commands; then
    commands
[elif commands; then
    commands...]
[else
    commands]
fi
```

### Exit Status

Commands issue a value to the system when they terminate, which is an integer in `0...255` indicating the success or failure of the command's execution (where `0` indicates success and others indicate failure)

```shell
$ ls -d /usr/bin
/usr/bin
$ echo $?
0
$ ls -d /bin/usr
ls: cannot access /bin/usr: No such file or directory
$ echo $?
2
```

Here `ls` is used twice, where the first time it executes successfuly (exit `0`), and the second fails (exit `2`)

---

## Using `test`

The `test` command performs a variety of checks and comparisons and has two forms:
`test expression` or `[ expression ]`

### `test` File Expressions

| Expression | Is true if |
|:-----------|:-----------|
| file1 -ef file2   | file1 and file2 have the same inode numbers (the two filenames refer to the same file by hard linking) |
| file1 -nt file2   | file1 is newer than file2 |
| file1 -ot file2   | file1 is older than file2 |
| -b file           | file exists and is a block-special (device) file |
| -c file           | file exists and is a character-special (device) file |
| -d file           | file exists and is a directory |
| -e file           | file exists |
| -f file           | file exists and is a regular file |
| -g file           | file exists and is set-group-ID |
| -G file           | file exists and is owned by the effective group ID |
| -k file           | file exists and has its “sticky bit” set |
| -L file           | file exists and is a symbolic link |
| -O file           | file exists and is owned by the effective user ID |
| -p file           | file exists and is a named pipe |
| -r file           | file exists and is readable (has readable permission for the effective user) |
| -s file           | file exists and has a length greater than zero |
| -S file           | file exists and is a network socket |
| -t fd             | fd is a file descriptor directed to/from the terminal. This can be used to determine where standard input/output/error is being redirected |
| -u file           | file exists and is setuid |
| -w file           | file exists and is writable |
| -x file           | file exists and is executable |

### String Expressions

| Expression | Is true if |
|:-----------|:-----------|
| string     | string is not null |
| -n string  | The length of string is greater than zero |
| -z string  | The length of string is zero |
| string1 = string2 or string1 == string2 | string1 and string2 are equal |
| string1 != string2 | string1 and string2 are not equal |
| string1 > string2 | string1 sorts after string2 |
| string1 < string2 | string1 sorts before string2 |

> warning: The `>` and `<` operators must be quoted or escaped when working with `test` if not, they will be interpreted by shell as redirect operators

```shell
if [ -z "$ANSWER" ]; then
    echo "There is no answer." >&2
    exit 1
fi
if [ "$ANSWER" = "yes" ]; then
    echo "The answer is YES."
elif [ "$ANSWER" = "no" ]; then
    echo "The answer is NO."
elif [ "$ANSWER" = "maybe" ]; then
    echo "The answer is MAYBE."
else
    echo "The answer is UNKNOWN."
fi
```

### Integer Expressions

| Expression | Is true if |
|:-----------|:-----------|
| integer1 -eq integer2 | integer1 is equal to integer2 |
| integer1 -ne integer2 | integer1 is not equal to integer2 |
| integer1 -le integer2 | integer1 is less than or equal to integer2 |
| integer1 -lt integer2 | integer1 is less than to integer2 |
| integer1 -ge integer2 | integer1 is greater than or equal to integer2 |
| integer1 -gt integer2 | integer1 is greater than to integer2 |

```shell
if [ -z "$INT" ]; then
    echo "INT is empty." >&2
    exit 1
fi
if [ $INT -eq 0 ]; then
    echo "INT is zero."
else
    if [ $INT -lt 0 ]; then
        echo "INT is negative."
    else
        echo "INT is positive."
    fi
    if [ $((INT % 2)) -eq 0 ]; then
        echo "INT is even."
    else
        echo "INT is odd."
    fi
fi
```

---

## A More Modern Version of `test`

Recent versions of `bash` include a compound command that acts as an enhanced replacement for `test`

`[[ expression ]]`

Where `expression` evaluates to either a `true` or `false` result

`[[ ]]` is similar to `test` but adds a new string expression

`string =~ regex`

Which returns true if `string` is matched by the regular expression `regex`

```shell
INT=-5
if [[ "$INT" =~ ^-?[0-9]+$ ]]; then
    echo "INT is an integer"
else
    echo "INT is not an integer" >&2
    exit 1
fi
```

`bash` also provides the `(( ))` compound command, useful for operating on integers.
Used to perform *arithmetic truth tests*, where the evaluation is non-zero

```shell
$ if ((1)) then echo "It is true"; fi
It is true
$ if ((0)) then echo "It is true"; fi
$
```

---

## Combining Expressions

It's possible to combine expressions by using logical operators

| Operation | test  | `[[ ]]` and `(( ))`   |
|:----------|:------|:----------------------|
| AND       | -a    | `&&`                  |
| OR        | -o    | `||`                  |
| NOT       | !     | `!`                   |

```shell
MIN_VAL=1
MAX_VAL=100
INT=50

if [[ "$INT" =~ ^-?[0-9]+$ ]]; then
    if [[ INT -ge MIN_VAL && INT -le MAX_VAL ]]; then
        echo "$INT is within $MIN_VAL to $MAX_VAL"
    else
        echo "$INT is out of range"
    fi
else
    echo "INT is not an integer" >&2
    exit 1
fi
```

## Control Operators: Another Way to Branch

`bash` provides two control operators `&&` and `||`

- `command1 && command2`
    `command2` is only executed if `command1` **is successful**
- `command1 || command2`
    `command2` is only executed if `command1` **is unsuccessful**

---

## Flow Control: Looping with While and Until

### `while`

Keeps looping while the expression evaluates to `0`

`while commands; do commands; done`

```shell
count=1

while [ $count -le 5 ]; do
    echo $count
    count=$((count + 1))
done
echo "Finished"
```

`break` command immediately terminates a loop and `continue` comand skips the remainder of the loop, by starting the next iteration of the loop

### `until`

Much like `while`, but will keep looping while the expression evaluates to `1`

`until commands; do commands; done`

---

## Flow Control: Branching with `case`

`case` is a multiple-choice compound command

```shell
case word in
    [pattern [| pattern]...) commands;;]...
esac
```

```shell
case $REPLY in
    0)  echo "Program terminated"
        exit
        ;;
    1)  echo "Hostname: $HOSTNAME"
        ;;
    *)  echo "Invalid entry" >&2
        exit 1
        ;;
esac
```

### `case` Pattern Examples

| Pattern   | Description |
|:----------|:------------|
| `a)`      | Matches if `word` equals `a` |
| `[[:alpha:]])` | Matches if `word` is a single alphabetic character |
| `???)`    | Matches if `word` is exactly three characters long |
| `*.txt)`  | Matches if `word` ends with the characters `.txt` |
| `*)`      | Matches any value of `word`. It is good practice to include this as the last pattern in a case command to catch any values of `word` that did not match a previous pattern |

### Combining Multiple Patterns

```shell
echo "
Please Select:
A. Display System Information
Q. Quit
"
read -p "Enter selection [A or Q]"

case $REPLY in
    q|Q)    echo "Program terminated"
            exit
            ;;
    a|A)    echo "Hostname: $HOSTNAME"
            ;;
    *)      echo "Invalid entry" >&2
            exit 1
            ;;
esac
```

---

## Positional Parameters

### Accessing the Command Line

The shell provides a set of variables that contain individual words on the command line they are named `0` throught `9`

`$0, $1, ... $9`

> To access variables from greater indices use `${number}`

Number of arguments `$#`

#### `shift` - Getting Access to Many Arguments

When receiving a large number of arguments, `shift` can be used to move shift the variables down `${n} = ${n+1}`, `$#` also decreases count by 1 every time `shift` is executed

`$*` expands into the list of positional parameters, starting with `1`, when surrounded by double quotes, it expands into a double-quoted string containing all the positional parameters



`$@` expands into the list of positional parameters, starting with `1`, when surrounded by double quotes, it expands each positional parameter into a separate word surrounded by double quotes