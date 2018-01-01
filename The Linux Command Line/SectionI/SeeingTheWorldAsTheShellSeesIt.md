# Seeing the World as the Shell Sees It

[Prev](Pipelines.md) | [Next]()

Each time a command line is typed, `bash` performs several processes upon the text before it carries out the command
The process that makes this happen is called *expansion*, something enters and it is expanded into something else before the shell acts upon it

`echo this is a test` -> `this is a test`
It prints out it's text arguments on standard output

`echo *` -> `Desktop Documents Music Pictures Public Templates Videos`
It didn't print `*` since it's considered a wildcard
The shell expands `*` into something else (in this case, the names of the files in the current working directory), before `echo` command is executed

## Expansions

### Pathname Expansion

The mechanism by which wildcards work is called *pathname expansion*

`ls` -> `Desktop Documents Music Pictures ...`
`echo D*` -> `Desktop Documents`
`echo *s` -> `Documents Pictures Templates Videos`
`echo [[:upper:]]*` -> `Documents Pictures Templates Videos ...`
`echo /usr/*/share` -> `/usr/kerberos/share /usr/local/share`

### Tilde Expansion

When used at the beginning of a word it expands into the name of the home directory of the named user, or if no user is named, the home directory of the current user

`echo ~` -> `/home/me`
`echo ~foo` -> `/home/foo`

### Arithmetic Expansion

The shell allows arithmetic to be performed by expansion

> `$((expression))`

`echo $((2 + 2))` -> `4`

Arithmetic expansion supports only **integers**

| Operator  | Description |
|:----------|:------------|
| +         | Addition |
| -         | Subtraction |
| *         | Multiplication |
| /         | Division (result will be in integer) |
| %         | Modulo |
| **        | Exponentiation |

### Brace Expansion

Allows creation of multiple text strings from a pattern containing braces

`echo Front-{A,B,C}-Back` -> `Front-A-Back Front-B-Back Front-C-Back`

Patterns to be brace expanded may contain a leading portion called a *preamble* and a trailing portion called *postscript*
The brace expression itself may contain either a comma-separated list of strings or a range of integer or single characters (the pattern may not contain whitespace)

`echo Number_{1..5}` -> `Number_1 Number_2 Number_3 Number_4 Number_5`
`echo {Z..A}` -> `Z Y X W V U T S R Q P O N M L K J I H G F E D C B A`
`echo a{A{1,2},B{3,4}}b` -> `aA1b aA2b aB3b aB4b`

### Parameter Expansion

`echo $USER` -> `me`

`printenv | less` Prints a list of available variables

### Command Substitution

Allows to use the output of a command as an expansion

`echo $(ls)` -> `Desktop Documents Music Pictures Public Templates Videos`
`ls -l $(which cp)` -> `-rwxr-xr-x 1 root root 71516 2012-12-05 08:58 /bin/cp`

## Quoting

`echo this is a      test` -> `this is a test`
*Word splitting* by the shell removed extra whitespace from the `echo` command's list of arguments

`echo The total is $100.00` -> `The total is 00.00`
Parameter expansion substituted an empty string for the value of `$1` because it was an undefined variable

### Double Quotes

If you place text inside double quotes `"` all the special characters used are treated as ordinary characters
The exceptions are `$`, `\` and \`

`ls -l two words.txt` -> `No such file`
`ls -l "two words.txt"`

```shell
$ echo "$USER $((2+2)) $(cal)"
me 4 January 2018
    January 2018
Su Mo Tu We Th Fr Sa
    1  2  3  4  5  6
 7  8  9 10 11 12 13
14 15 16 17 18 19 20
21 22 23 24 25 26 27
28 29 30 31
```

```shell
$ echo $(cal)
January 2018 Su Mo Tu We Th Fr Sa 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30 31
```

Resulted in a command line (`echo`) containing 38 arguments

```shell
$ echo "$(cal)"
    January 2018
Su Mo Tu We Th Fr Sa
    1  2  3  4  5  6
 7  8  9 10 11 12 13
14 15 16 17 18 19 20
21 22 23 24 25 26 27
28 29 30 31
```

Command line with 1 argument that includes the embedded spaces and newlines

### Single Quotes

They suppress **all** expansions

### Escaping Characters

To quote only a single character it's possible by escaping it

`echo "The balance for user $USER is: \$5.00"` -> `The balance for user me is: $5.00`

[Prev](Pipelines.md) | [Next]()