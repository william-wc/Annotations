# Regular Expressinos

Regular expressions are symbolic notatinos used to identify patterns in text

## `grep` - Search Through Text

`ls /usr/bin | grep zip`
List all the files in `/usr/bin` whose names contain substring `zip`

`grep [options] regex [file...]`

### `grep` Options

| Option | Description |
|:-------|:------------|
| --ignore-case, -i         | Ignore case, do not distinguish between upper and lowercase characters |
| --invert-match, -v        | Invert Match, Normally `grep` prints lines that contain a match, this options prints all lines that do not contain a match |
| --count, -c               | Print the number of matches (or non-matches with `-v`) |
| --files-with-matches, -l  | Print the name of each file that contains a match instead of the lines themselves |
| --files-without-match, -L | Like `-l` but print only the names of files that do not contain matches |
| --line-number, -n         | Prefix each matchign line with the number of the line within the file |
| --no-filename, -h         | For mulitfile searches, suppress the output of filenames |

## Metacharacters and Literals

