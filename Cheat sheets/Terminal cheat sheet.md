---
alias: Shell cheat sheet
---

# Bash

## Hotkeys

- <kbd>Ctrl+r</kbd> and <kbd>Ctrl+s</kbd>: search command history backwards/forwards
- <kbd>Ctrl+a</kbd>: move to beginning of line
- <kbd>Ctrl+e</kbd>: move to end of line
- <kbd>Ctrl+u</kbd>: clear prompt
- <kbd>Ctrl+k</kbd>: clear from cursor to end of prompt

## Basics

- Variable substitution (`$NAME`) happens in double quotes or when unquoted, but not in single quotes

## Operators & Functions

- `!!`: repeat the last command entered (ex. `sudo !!`)
- `!$`: last item in the last command (ex. `foo.txt` in `cat foo.txt`)
- `$?`: exit code of the last command

---

- `>`: write file on right with stdout of command on left
- `>>`: same as `>`, but appends
- `<`: use file on right as stdin for command on left
- `|`: pipe stdout of command on left to stdin of command on right

---

- `a && b`: do `b` if `a` succeeded
- `a || b`: do `b` if `a` failed

---

- `$(command)`: command substitution (substitutes the result of the command)
- `${param}`: parameter substitution (substitutes the parameter value)
- `[[ condition ]]`: conditional test (the spaces are important)
    - 0 is true and 1 is false (matches exit codes)
    - `=`: string comparison
    - `-eq`: numeric comparison (also: `-lt`, `-le`, `-gt`, `-ge`, `-ne`)
    - `-f`: test if something is a file

---

- function parameters: `$1`, `$2`, etc
- `"$@"`: all parameters
- `$#`: number of parameters given

---

- for loop over every file in working directory:

```bash
for i in *; do echo "$i"; done
```

- if statement:

```bash
if [[ condition ]]; then
	echo "condition 1 passed"
elif [[ condition2 ]]; then
	echo "condition 2 passed"
else
	echo "both conditions failed"
fi
```

## Wildcards

- `cp m*.txt scifi/`
- `*/` matches only folders
- `file ^*foo*`: exclude files containing foo
    - requires `setopt extendedglob`

# Fish

## Basics

- `set -x VARIABLE SomeValue`: export a variable
- `alias --save name="definition"`: save alias
- `>?`: redirect output to file, but avoid overwriting existing file
- `&>`: redirect stdout & stderr to the same place
- command substitution: `(command)`
- `command &`: run command in background
    - <kbd>Ctrl+Z</kbd>: suspend execution & return to fish
    - `fg`: go back to suspended command
    - `bg`: send suspended command to background
- access arguments with `$argv[1]` instead of `$1`
- `$status`: holds last exit code
- `if type -q command ...`: check if command exists

## History recall

- <kbd>Alt+Up Arrow</kbd>: recall individual arguments
- <kbd>Alt+s</kbd>: recall last command with `sudo` prefixed

## String matching

- `string match 'word' 'string1' 'string2'... `
    - `-a`: print all matches
    - `-e`: print entire string, not just match
    - `-i`: case insensitive
    - `-n`: report each match as a 1-based start position and length
    - `-r`: interpret as Perl-style regex
        - without `-r` or `-e`, the pattern must match the entire string
        - if the regex contains capturing groups, multiple items will be reported for each match, one for the entire match and one for each capturing group
        - *automatically sets variables for all named capturing groups* `(?<name>expression)`
            - if `-a` is used, each variable will contain a list of matches
    - `-v`: only print lines that do not match the pattern

## Snippets

Get name of frontmost app (Mac):

```shell
set result (string match -r '"LSDisplayName"="(.+)"' (lsappinfo info -only name (lsappinfo front)))
echo $result[2]
```

# Utilities

## Files & folders

- `ls -a`: list hidden files
- `ls -t`: sort by modified date
- move/copy multiple files: `cp a.txt b.txt c/`
- `file`: show information about file

## find

- `find dir -type f -name foo*`: find all *files* starting with "foo" in dir (recursive)
    - use `-type d` for folders (directories)
- `find dir -name .DS_Store -delete`: delete all *.DS_Store* files in dir
- `find . -name foo* -exec command`: ==execute `command` on all matching files==

## lsof (list open files)

- `lsof +d dir`: shows open files in dir
- `lsof +D dir`: shows open files in dir and its subfolders
- `lsof -c Finder`: list all files Finder has open
- `lsof -i :80`: find out which processes are using port 80

## Text manipulation

- `wc`: output number of lines, words, and characters (in that order)
- `less`: show file contents (scrollable)
- `head -#`: output first # lines
- `tail -#`: output last # lines
- `sort`: sort alphabetically (doesn't modify original)
    - `-r` to reverse, `-h` for numeric sorting
 - `uniq`: filter out duplicate lines **if they are adjacent**
    - combine `sort` and `uniq` to get all duplicates
- `grep`: match regex pattern (see [[#grep]])
- `sed`: modify stdin based on regex
    - ex. `sed 's/foo/bar/' file.txt`
    - end regex with `/g` for global search

## grep

- `grep 'word' filename`
    - filename can also be a directory or pattern, like `.` or `*.js`
    - `fgrep` (or `grep -F`) is faster but finds only fixed patterns (not regexp)
    - `egrep` (or `grep -E`) handles extended regular expressions
    - `grep -P` uses Perl-style regexps (allows things like look-around assertions)
- `-i`: case insensitive
- `-w`: whole word matches
- `-v`: invert match (match only lines that do not contain the word)
- `-R`: search directory recursively (ex. `grep -R foo ~/bar`)
    - `-l`: show matching filenames
    - `-h`: do not output file names

### Output configuration

- display surrounding lines: `-B` (before) and `-A` (after)
- print only what matches pattern: `-o`
- count number of matches: `-c`
    - prepend line number: `-n`

## nano

- <kbd>Ctrl+Shift+6</kbd>: mark text
    - <kbd>Ctrl+k</kbd>: cut/delete marked text

## tmux

- `tmux`: create a new session
- `tmux attach`: reattach to existing session

### Keyboard shortcuts

- <kbd>Ctrl+b "</kbd>: split horizontally
- <kbd>Ctrl+b %</kbd>: split vertically
- <kbd>Ctrl+b {arrow}</kbd>: move focus
- <kbd>Ctrl+b+{arrow}</kbd>: resize focused pane
- <kbd>Ctrl+b x</kbd>: close pane (y to confirm)
- <kbd>Ctrl+b d</kbd>: detach from session

<div class="rich-link-card-container"><a class="rich-link-card" href="https://www.tecmint.com/tmux-to-access-multiple-linux-terminals-inside-a-single-console/" target="_blank">
	<div class="rich-link-image-container">
		<div class="rich-link-image" style="background-image: url('https://www.tecmint.com/wp-content/uploads/2016/01/Tmux-Manage-Multiple-Linux-Terminals.png')">
	</div>
	</div>
	<div class="rich-link-card-text">
		<h1 class="rich-link-card-title">How to Use 'Tmux Terminal' to Access Multiple Terminals Inside a Single Console</h1>
		<p class="rich-link-card-description">
		tmux (short for Terminal MUltipleXer), a simple and modern alternative to the well-known GNU screen, and will enable you to access and control a number of terminals (or windows) from a single terminal.
		</p>
		<p class="rich-link-href">
		https://www.tecmint.com/tmux-to-access-multiple-linux-terminals-inside-a-single-console/
		</p>
	</div>
</a></div>

## crontab

- `crontab -l`: print crontab
- `crontab -e`: edit crontab (auto reloads on exit)
    - format: `minute hour dayOfMonth month dayOfWeek command`
    - separate multiple times with commas, ex `1,2,3`
    - use `*/#` to specify an interval, ex. `*/20 * * * * command` means "every 20 minutes" (without the \*/ this would mean "on minute 20 of every hour")
    - `@reboot command` to run on reboot
- using `sudo` edits a separate crontab for `root`

## rsync

- `rsync -av dir1 dir2`: copy directory 1 to directory 2
    - add a trailing slash on dir1 to copy the contents of the directory, not the directory itself
    - `-a` (archive): sync recursively and preserve symlinks and metadata
    - `-v`: verbose
- `rsync -azP dir1 username@host:/path/to/dir2`: copy over SSH
    - `-z`: add compression
    - `-P`: show progress bar, and allow resuming transfers
- other options:
    - `--exclude=pattern`: exclude files matching `pattern`
    - `-n`: dry run
- rsync uses modification times to sync only changed files

## npm

- `npm list`: list installed packages (useful for [[#grep]]ping)
    - `-g`: global
    - `-a`: include nested dependencies
    - `-l`: include descriptions

## ffmpeg

- `ffmpeg -i input.mp4 -ss 00:01:40 -to 00:02:16 -c copy output.mp4`: trim to between 1m:40s and 2m:16s
- `ffmpeg -i input.mp4 -ss 00:00:14.435 -vframes 1 out.png`: output one frame from 0h:0m:14sec:435msec to *out.png*
- `ffmpeg -i input.m4a -c:v copy -c:a libmp3lame -q:a 4 output.mp3`: convert m4a to mp3 without significant quality loss

## youtube-dl

- `--simulate`: dry run
- `--match-title keyword`: only download items matching *keyword*
- `-F url`: list all available formats
    - `-f # url`: download video with format number
- `-f bestaudio[ext=m4a] --add-metadata --embed-thumbnail url`: save audio with thumbnail and chapter markers
- `-o "%(playlist_index)s-%(title)s.%(ext)s" <playlist_link>`: save a playlist with numbered files

## jq

- pipe JSON (ex. from `curl`) into `jq` with no arguments to pretty-print
- `jq -s '.[0] * .[1]' --indent 4 old.json patch.json > new.json`: recursively merge *patch.json* into *old.json*

## pandoc

- basic conversion

```shell
pandoc file.docx -o file.html
```

# PowerShell

- `explorer .`: open Explorer window in current working directory
    - or `ii .`: short for `Invoke-Item .`
- `taskkill /F /IM name`: kill process matching name (wildcard * can be used)
- `New-Item -ItemType SymbolicLink -Path "dest" -Target "src"`: create symbolic link to *src* at *dest*
    - must be elevated
    - `-ItemType HardLink` for hard links
- `Get-Content urls.txt | ForEach-Object {Invoke-WebRequest $_ -OutFile $(Split-Path $_ -Leaf)}`: download list of URLs
    - `Split-Path $_ -Leaf` gets the last part of the URL as the file name (doesn't work on weird URLs like Twitter images)

## Snippets

Delete hidden macOS files:

``` powershell
$theSource = "E:\"       # <<<<< insert drive here
Get-Childitem $theSource -Include @("._*", ".DS_Store", ".fseventsd", ".TemporaryItems", ".Trashes", ".Spotlight-V100") -Recurse -Force -ErrorAction SilentlyContinue | Remove-Item -Force -Recurse
```

# See also

- [[The Terminal • furbo.org]]
