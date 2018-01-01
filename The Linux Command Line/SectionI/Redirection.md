# Redirection

[Prev](WorkingWithCommands.md) | [Next](Pipelines.md)

**I/O redirection** which means redirecting the input and output of commands to and from files, as well as connect multiple commands to amke powerful command *pipelines*

| Command   | Description |
|:----------|:------------|
| cat       | Concatenate files |
| sort      | Sort lines of text |
| uniq      | Report or omit repeated lines |
| wc        | Print newline, word and byte counts for each files |
| grep      | Print lines matching a pttern |
| head      | Output the first part of a file |
| tail      | Output the last part of a file |
| tee       | Read form standard input and write to standard output and files |

## Standard Input, Output and Error

Many of the programs produce output of some kind
They consist mostly of two types:

- The program's results, the data the program is designed to produce
- Status and error messages about the program

Programs as `ls` actually send their results to a special file caleld *standard output (stdout)* and their status messages to another file called *standard error (stderr)*. By default both are linked to the screen and not saved into a disk file

In addition, many programs take input from a facility called *standard input (stdin)*, which is, by default, attached to the keyboard

I/O redirection allows us to change where output goes and where input comes from

### Redirecting **Standard Output**

To redirect **standard output** to another file we use the `>` operator followed by the name of the file
`ls -l /usr/bin > ls-output.txt`

`ls -l /bin/usr > ls-output.txt`
This causes an error which is sent to **standard error** (currently the screen)
And clears any value from `ls-output.txt`

To append output instead of overwriting the file from the beginning:
`ls -ls /bin/usr >> ls-output.txt`

### Redirecting **Standard Error**

Redirecting standard error lacks the ease of using a dedicated redirection operator
So it should be done by refering to it's *file descriptor*

A program can produce output on any of several numbered file streams
The shell references standard input, output and error as file descriptors 0, 1 and 2 respectively

`ls -l /bin/usr 2> ls-error.txt` <- The file descriptor 2 is placed immediately before the redirection operator to perform the redirection of standard error

### Redirecting Standard Output and Standard Error to One File

There are two ways to do this

1. `ls -l /bin/usr > ls-output.txt 2>&1`
    This is the traditional way, works with older versions of the shell
    First redirect *standard output* to the file `ls-output.txt`, then redirect file descriptor 2 (*standard error*) to file descriptor 1 (*standard output*) using notation `2>&1`
    Order of redirection is significant. Redirection of *standard error* must always occur **after** redirecting standard output or it won't work
2. `ls -l /bin/usr &> ls-output.txt`
    Recent versions of *bash* provide a more streamlined method
    In this single notation `&>` redirects both standard output and standard error to file `ls-output.txt`

### Disposing of Unwanted Output

The system provides a way to redirecting output to a special file called `/dev/null`. This file is a system device called *bit bucket*, which accepts input and does nothing with it

`ls -l /bin/usr 2> /dev/null` <- Supresses error messages

### Redirecting Standard Input

`cat` - Concatenate Files

Reads one or more files and copies them to standard output

`cat [file...]`
`cat ls-output.txt` <- displays the contents of the file `ls-output.txt`

But in the absence of filename arguments, `cat` copies standard input to standard output

`cat > lazy_dog.txt && The quick brown fox jumped over the lazy dog.`
Creates a file `lazy_dog.txt` containing the followed text

`cat < lazy_dog.txt`
Using `<` redirection operator, we change the source of standard input form keyboard to the file `lazy_dog.txt`

[Prev](WorkingWithCommands.md) | [Next](Pipelines.md)