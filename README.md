# zsh little dev

<a href="http://www.youtube.com/watch?feature=player_embedded&v=nl2ooOxDCjY
" target="_blank"><img src="http://img.youtube.com/vi/nl2ooOxDCjY/0.jpg"
alt="Tim" width="400" height="260" border="10" /></a>

[Reflow to md from zapper with extra broken](http://www.zzapper.co.uk/zshtips.html)

Here be a smorgasboard of random zsh notes.

# Global aliases
Alias                    | Description
------------------------ | ----------------
`alias -g ND='*(/om[1])'` | newest directory
`alias -g NF='*(.om[1])'` | newest file

Example of use
```
$ cp NF ND          # copy newest file to newest directory
$ cat NF > $(print NF).txt
```

# Listing
Command                    | Description
-------------------------- | ------------
`ls *(.)`                  | list just regular files
`ls -d *(/)`               | list just directories
``ls (x*`x[3-5])``           | list files x* except x3 to x5
`ls x^[3-5]*`              | list files x\* except x3 to x5
`ls *(.[3])`               | third file
``ls **/*`*/.git/*``         | ignore all git subdirectories ``*`*`` matches a path
`vi *(.om[1])`             | vi newest file
``vi *`ssver.scc(.om[1]) &`` | newest file ignoring any vssver.scc
`vi !$`                    | vi last parameter
`vi !-2:2`                 | second parameter of second but last command
`vi !$:r.py`               | vi last parameter but change extension to .py
`^php^cfm`                 | modify previous command (good for correcting spellos)
`ls *(.L0)`                | list pesky empty files (yes that is a zero)
`ls -l *(L-2)`             | list file size less than 2 bytes
`ls -l *(.L-20)`           | list file size less than 20 bytes - . ignore directories

# Listing by file size
Command                    | Description
-------------------------- | ------------
`ls -l *(Lk+100)`                | list file size larger/greater than 100kb
`ls -l *(Lm+2)`                  | list file size larger/greater than 2 mbs
`ls **/*(.Lm+10)`                | list files larger than 10MB anywhere in hierarchy
`ls -hlS **/*(.Lm+2) | less`     | list largest files  largest first
`ls -hlS /**/*(.OL[1,10])`       | find the 10 biggest files on your system
`ls *(.m0)`                      | modified today (last 24 hours)
`ls *(.m-1)`                     | modified today (last 24 hours)
`ls *(.^m0)`                     | not modified today
`ls *.*(m3)`                     | modified 3 days ago
`ls *.*(mh3)`                    | modified 3 hours ago
`ls *.*(mh-3)`                   | less than 3 hours
`ls *.*(mh+3)`                   | more than 3 hours
`ls *.*(^mh3)`                   | all files not 3 hours  old
`mv *(.mw+2) old/`               | older than 2 weeks
`mv *(.mM+2) old/`               | older than 2 months

# Listing by permission
Command                                   | Description
------ | ----------
`ls -ld *.*(u:apache:)`                   | User is apache
``ls -l *.*`[a-m]*(u:nobody:g:apache:.xX)`` | Excluding files a-m but only if owned by apache
`ls **/*(u0WLk+10m0)`                     | Find all files owned by root (u0), world-writable (W), more than 10k in size (Lk+10) and modified during the last hour (m0)
`ls **/*(.:g-w:)`                         | find all files that don’t have the write permission to group in current directory and all subdirectories


# Counting (#)
Requires extended globbing `setopt EXTENDED_GLOB`

Command                                     | Description
------------------------------------------- | --------------------------------------------
``grep -i "$1" */*.php`libs/*`temp/*`test/*`` | Exclude directories lib,temp,test from grep
`ls DATA_[0-9](#c3).csv`                    | match all files DATA_nnn.csv
`ls a(#c3).txt`                             | match aaa.txt
`ls DATA_[0-9](#c4,7).csv`                  | match DATA_nnnn.csv to DATA_nnnnnnn.txt
`ls DATA_[0-9](#c4,).csv`                   | match DATA_nnnn.csv to DATA_nnnnn.txt etc
`ls DATA_[0-9](#c,4).csv`                   | match DATA_n.csv to DATA_nnn.txt
`touch {1..5} {6,7,8,12} {00..03}`          |
`touch {t,p}{01..99}.{php,html,c}`          | generate 600 test files

# Greps
Command | Description |
------- | -------------|
``grep -i "$1" **/*.{js,php,css}`(libs|temp|temp|test)/* `` | exclude directories from grep  EXTENDED_GLOB required|
``grep -iw '$direct' report/**/*.{inc,php}``   | searching for a php variable|

# Cleanup
| Command        | Description                             |
|--------------- | ----------------------------------------|
| `rm **/.*.swp` | deleting  double dot files & swap files |


# Use tab to complete/display history item before executing


Command | Description |
------- | -------------|
`!1 `| oldest command in your history
`!!` | previous command
`!-2` | command before last
`!$` | last argument of previous command
`!$:h`  | last argument, strip one level
`!$:h:h`  | last argument, strip two levels
`!?echo` |
`echo !* !!:* `| all parameters
`echo !$ !!:$`  | last parameter
`echo !^ !:1 !!:1` | first previous parameter
`echo !:2-3`   | echo previous parameters 2 to 3
`echo !:2*`    | echo previous parameters 2 onwards
`echo !:2-`    | echo previous parameters 2 onwards omitting last
`echo !:-3`    | echo first 3 previous parameters
`echo !-2:2` | second parameter of second but last command
`touch 1 2 3`    |
`!!:0 !^ !:2 !$ !#$ !#:2 !#1 !#0`   |
`history`               | View recent commands
`!42`                   | Re-execute history command 42

# substitute previous command
Command | Description |
------- | -------------
r oldstr=newstr |
!!:s/fred/joe/        | edit previous command replace first fred by joe
!!:s/fred/joe/        | Note : sadly no regexp available with :s///
!!:gs/fred/joe/       | edit previous command replace all fred by joe
mv Licence\ to\ Print\ Money.pdf !#^:gs/\\ //  | rename file removing spaces
^fred^joe             | edit previous command replace fred by joe
^str1^str2^:u:p       | replace str1 by str2 change case and just display
echo chim |
^chim^&-&ney-&-&-cheree | reuse LHS
!42:p | also use control-R
^str1^str2^:G         | replace as many as possible

In all of above remember <TAB> will display changed command WITHOUT executing it

Command | Description
------- | ------------
cd !?ls<TAB>   | get command and parameters of a previous ls command
cd !?ls?:\*<TAB>    | get (just) parameters of a previous ls command

Command | Description |
------- | -------------|
function scd(){setopt nonomatch;e=/dev/null;cd $1 &> $e||cd ${1}* &> $e||cd *$1 &> $e||cd *${1}* &> $e||echo sorry} |
function ddump(){diff -w \`dump/"$1" "$1"}   |  diff local file with new one in dump
function cdump(){cp -p \`dump/"$1" "$1"}   |  replace local file with new one in dump


# Generating a command from an earlier one
How to recall the parameters of a previous command, on line 7 below recall the parameters of line 5
``` zsh
5> mv somefile1 /home/saket/stuff/books/
6> acroread somefile.pdf
7> mv somefile2 /home/saket/stuff/books/
```

``` $ mv !?saket<TAB>``` Would bring up the whole line ready for a little editing
``` $ mv !?saket?:*<tab>``` Would just bring up the parameters
If you know the history number of the line (say 5) with desired parameters you can try ```!5:s/somefile1/somefile2/```
and if you dont know the history number```!?saket?:s/somefile1/somefile2/```

# Variable Substitution

Truncate strings in an array
```
s=(fred joe peter);echo ${s/(#m)*/$MATCH[1,3]}
```

# History Substitution Summary
For CURRENT line that you are editing (the # designates current line)

Remember Tab will expand the following:

Command | Description |
------- | -------------|
!#:0   |  command
!#^    |  first parameter
!#:1   |  first parameter
!#:1-4 |  first 4 parameters
!#$    |  last parameter
!#*    |  all parameters
!#$:s/bash/zsh  | perform substitution on previous parameter

# Backup a file with a prefix

``` zsh
cp longfilename.php backup_!#^
cp {,backup_}longfilename.php   # same thing
```

# Backup a file with a suffix

``` zsh
cp longfilename.php !#^:r.bak
```
expands to cp longfilename.php longfilename.bak

```
cp longfilename.{php,bak}
```

# For Previous Command (for comparison)
Command | Description |
------- | -------------|
!-1   |   repeat whole command
!!    |   repeat (shortcut)
!!0   |   command
!^    |   first parameter
!:1   |   first parameter
!:1-4 |   first 4 parameters
!:-4  !:0-4 |  first 4 parameters plus command
!!-   |  all but last parameter
!51$  |  last parameter of history entry 51
!$    |  last parameter
!*    |  all parameters
!!:s/bash/zsh | or ^bash^zsh
!^:t   |  just file name of first parameter
!$:h   |  just path of last parameter
!-2$:r |  just file name without extension of first parameter

For last but one command
Command | Description |
------- | -------------|
!-2    | repeat last but one command
!-2^   | first parameter last but one command
!-2$   | last parameter last but one command
!-2:2  | second parameter of second but last command
!-2:s/bash/zsh | etc

For history command 42
`!42`


!:0 is the previous command name
!^, !:2, !:3, !$ are the arguments
!* is all the arguments
!-2, !-3,  are earlier commands
!-2^, !-2:2, !-2$, !-2* are earlier parameters

# recall/step through previous parameters
ls /
fred='/bin/path/fred.txt'
echo ${fred:e}
echo ${fred:t}
echo ${fred:r}
echo ${fred:h}
echo ${fred:h:h}
echo ${fred:t:r}
cd !$:h  (remove file name)

# cd to directory containing report.php

``` zsh
cd **/report.php(:h)
cat !!:t (only file name)
```

#  Convert images (foo.gif => foo.jpg):

``` zsh
$ for i in **/*.gif; convert $i $i:r.jpg
```

#  Examples of if then else conditionals

Command | Description
------- | -------------
[[ 0 = 0 ]] && echo eq \|\| echo neq |
[[ 1 = 0 ]] && echo eq  \|\| echo neq |
if [ $# -gt 0 ];then string=$\*;else;string=$(getclip);fi | get parameter OR paste buffer
var=133;if [[ "$var" = <-> ]] ; then echo "$var is numeric" ;fi
if [[ "$ip" = <-> ]] then | check ip address numeric
if [[ "$1" == [0-9] ]]  | if $1 is a digit
if (( $# == 0 )); |
if [ $# -gt 0 ]  | parameter cnt > 0 (arguments)
if [[ "$url" = www* ]] | begins with www
if [ "$p1" = "end" ] \|\| [ "$p1" = "-e" ] |
if [[ "$p2" == \*[a-zA-Z][a-zA-Z][a-zA-Z]\* ]]  | contains at least 3 letters
if builtin cd $1 &> /dev/null ; |
if [[ -e /c/aam/z$1 ]]  | file exists
if [ $cnt -eq 1 ] |
if (( ${#dirs} == 1 )); then   | count array length
if [[ "$pwd" == *$site2* ]] |

``` zsh
print ${param:&}   (last substitute)
```

[a-za-z]:

# Directory substitution
If you were in directory

``` zsh
# cd old new
/c/inetpub/dev.somehomes.co.uk/epsystem/eppigeon/
cd dev www
# would put you in parallel directory
/c/inetpub/www.somehomes.co.uk/epsystem/eppigeon/
```

# completion

`cd /v/w/h/<tab>`
expands to
`cd /var/www/html/`

#  filtering the output of a command conventionally

``` zsh
print $(history -n -1|sed 's/.* //')
```

#  `${${(z)foo}[2]}` zsh filtering mechanism

``` zsh
print ${${(z)$(history -n -1)}[-1]}
print ${${(z)history[$((HISTCMD-1))]}[-1]}
gvim.exe $(history -n -1 | sed "s/^[^ ]* //;s/ .*//")
print ${${(z)history[$((HISTCMD-1))]}[2]}
```

#  Save last 4 history items to a file (without numbers)
Command | Description
------- | -------------
fc -ln -4 > /tmp/hist   | no numbers
fc -ln 1 | grep rsync | gvim -
fc -l -5     | 5 most recent
fc -l 1 5   | 5 oldest
fc -l -10 -5  | 10th newest to 5 newest

#  ls
Command | Description |
------- | -------------|
`ls -ld *(/^F)` | list any empty sub-directories
`ls -ld **/*(/^F)` | recursively list any empty sub-directories
`print **/*(/^F) \| xargs -n1 -t rmdir`  | delete empty directories
`rmdir ./**/*(/od) 2> /dev/null` | deletes empty directories
`autoload zargs;zargs ./**/*.{php,inc,js} -- grep -i 'cons. unit'` |   EXTENDED_GLOB
`zargs **/*.{js,php,css}`(libs|locallibs|test|dompdf)/* -- grep console.log ` |  EXTENDED_GLOB
`zargs ./**/*.(php|inc|js) -- tar rvf dev2$(date '+%d-%m-%Y').tar ` |

#  grep whole file structure for php files with if ($var=4) (single equals) bug
detect if ($fred=2) type php errors (single equals)

``` zsh
zargs ./**/*.{inc,php} -- grep -i 'if *( *$[a-z0-9_]*=[0-9"]'
```

#  selectively tar a web root

``` zsh
zargs ./{html,live}/**/*.(php|inc|js)`(**/wiki|**/dompdf)/* -- tar rvf /tmp/web2$(date "+%d-%m-%Y").tar
zargs **/*.(php|inc) -- sed -i 's#ereg_replace("\([^"]*\)"#preg_replace("/\1/"#g'    ## global sed substitute using zargs
ls ^x*           # list all but x*
```

#  list all files without an extension (dotless)

``` zsh
a=(**/*(.D));echo $#a  # count files in a (huge) hierarchy
ls *`*.*(.)
```

#  Delete all directories Pictures_of_* except Pictures_of_beautiful_flowers

``` zsh
rm -rf Pictures_of_^beautiful_flowers   # selective delete
ls x*`(x3|x5)    # list files x* except x3 and x5
ls **/fred*`*junk*/* # list all files fred* unless in a junk directory
```

#  grep
Don't use egrep, grep -E is better, or even better `ag`
single quotes stop the shell, " quotes allow shell interaction

``` zsh
grep 'host' **/(*.cfm`(ctpigeonbot|env).cfm)
grep -i 'host' **/(*.cfm`(ctpigeonbot|env).cfm)`*((#s)|/)junk*/*(.)
egrep -i "^ *mail\(" **/*.php
grep "^ *mail\(" **/*.php`*junk*/*  #find all calls to mail, ignoring junk directories
```

grep '.' dot matches one character

``` zsh
grep b.g file    # match bag big bog but not boog
```

grep * matches 0 , 1 or many of previous character
grep "b*g" file # matches g or bg or bbbbg
grep '.*' matches a string
grep "b.*g" file # matches bg bag bhhg bqqqqqg etc

# grep break character is \

``` zsh
grep 'hello\.gif' file
grep "cat\|dog" file matches lines containing the word "cat" or the word "dog"
grep "I am a \(cat\|dog\)" matches lines containing the string "I am a cat" or the string "I am a dog"
grep "Fred\(eric\)\? Smith" file   # grep fred or frederic
```

# grep back references (memory)

``` zsh
grep -i "<H\([1-6]\).*</H\1>" *.html # matches pairs of tags
tel blenkinsop | grep -o "[[:alnum:][:graph:]]*@[[:alnum:][:graph:]]*" # filter just an email address from a text stream (not zsh)
```

#  ls

ls command | comment
--- | ---
`ls \*.h`(fred|foo).h` | same thing
`ls (x\*`x[3-5])  ` | list files x\* except x3 to x5
`ls *[^2].php`\*template\* ` | list files with 2nd filter
`ls (xx|yy)      ` | list xx or yy
`ls *.(jpg|gif)  ` | list graphic files
`ls fred{joe,sid}.pl` |
`ls fred{09..13}.pl` | range
`ls fred<76-88>.pl` | list all files fred76.pl to fred88.pl range
`ls fred<76->.pl ` | list all files fred76.pl to fred9999*.pl etc
`ls {_,}fred.php ` | list files _fred.php fred.php
`ls (_|)fred.php ` | same effect by globbing
`ls *.{jpg,gif}(.N)` | don't break if one or other image type absent
`# FNG optionally matching a character
`ls -l *y{2,}.cfm ` |  matches *y.cfm and *y2.cfm
`ls -l *y(2|).cfm ` |  matches *y.cfm and *y2.cfm
`ls *{y2,y}.cfm   ` |  matches *y.cfm and *y2.cfm
`ls *y2#.cfm      ` |  matches *y.cfm and *y2.cfm
`ls foot(fall)#.pl` |  match option string fall

``` zsh
setopt no_case_glob  # set ignore case for ls etc
zstyle ':completion:*' matcher-list 'm:{a-zA-Z}={A-Za-z}' 'r:|[._-]=* r:|=*' 'l:|=* r:|=*'   # case insensitive completion for cd etc
```

#  Globbing modifiers

Modifier | Description
--- | ---
:e | just the suffix  (Extension)
:r |  removes the suffix  (Remove)
:t |  takes away the path (Tail)
:h |  takes away one level (Head)
:a |  adds full Path (Add)
. | means must be regular files not directories etc
\*(om[1]) | picks most recently modified file
(.N) | no warning message if any file absent
ls (#i)\*.pmm    | case insensitive globbing (note exact syntax)
`ls *(.[2])`       | second file in list
`ls *(om[1]) `     | print the most recent file
`cp *(om[1])<TAB>` | will complete file name
`ls *(.om[1]) `    | print the most recent file (not directory)
`ls -l *(Om[1]) `  | oldest file
`mv  *(.om[2,$]) old/ `  | move all but newest file
`ls -lt **/*.txt(D.om[1,5])` | list 5 most recent files in hierarchy
`dirs=( '' **/*(DM/) ) eval 'ls ${^dirs}*(ND.om[1,5])' | list 5 most recent files in each sub-directory
`ls {^dev*,}/index.php(.N) `| ignore directories beginning dev*
`ls (dev*|fred*|joe*)/index* `| does not break if no matches
`ls **/index.php`dev*(/*)## `  | ignore subdirectories dev\* multi-level
`vi *(.om[1]^D) `  | vi newest file ^D means switch off GLOB_DOTS ie ignore dot files
`ls *.txt(.om[1]) ` | ls newest *.txt file
`ls -tld **/*(m-2) ` | list files modified in last 2 days in hierarchy
`ls *(.om[1,5])`    | print the 5 most recent files
`ls *(.Om[1,5]) `   | print the 5 oldest files
`ls -l *(m4) `     | list files modified exactly 4 days ago
`ls -ltd *(mw3) `  | list files 3 weeks old
`ls -1ld *([1,10]) ` | list just 10 files one per line , no directories
`ls *(m-1) `       | files modified today
`ls *(m0) `        | files modified today
`ls *(^m0) `       | files NOT modified today
`vi *(m0) `        | re-edit all files changed today!
`cp *.mp3(mh-4) /tmp `| copy files less than 4 hours old
`ls -ltd *(mh0) `   | list files modified only in last hour
`ls *.{aux,dvi,log,toc} `| rm latex temp files
`rm ./*(Om[1,-11]) `| removes all files but the ten newest ones (delete all but last 10 files in a directory)
`mv \*.\*(^m-1) old/ `| move all but today's files to sub-directory archive older files


#  exact dates (N)

Command | Description
--- | ---
`ls -l \*.\*(mM4) | 
`autoload -U age i`
`ls -tl *.*(e#age 2014/06/01 now#)` | 
`ls -tl *(.e#age 2014/06/01 2014/06/30#) ` |
`ls [01]<->201[45]/Daily\ report*.csv(e#age 2014/10/22 now#) `|
`files=(${(f)"$(ls \*$\*\*)"}(.N))` | store matching files
`ls *(n:t) `       | order by name strip directory
`ls **/*(On:t) `    | recursive reverse order by name, strip directory
`ls PHP*/**/*.php `| recursive but only for subdirectories PHP*
`ls *.c(:r) `      | strip suffix
`ls **/*(.) `      | only files no directories (.) means files only
`ls -ld *(/) `     | list only directories

#  Oddities

Command | Description
--- | ---
chmod g+w **/*  |  group write
[[ FOO = (#i)foo ]]  | case insensitive matching
fred=$((6**2 + 6))      | can do maths
: > /apache/access.log  | truncate a log file

#  arrays

``` zsh
X=(x1 x2)               # create an array
print -C 1 $X           # print each array element on it's own line
```

# 2D arrays lookup conversion

``` zsh
typeset -A convtable
convtable=(151 2 152 2 153 2 158 4 159 3 160 2 171 4 172 1 173 4)
echo $convtable[158]
print ${#path}          # length of "path" array
print ${#path[1]}       # length of first element in path array
print ${$( date )[2,4]} # Print words two to four of output of ’date’:
array=(`/.zshenv `/.zshrc `/.zlogout)
filelst[$(($#filelst+1))]=$x # append (push) to an array
filelst+=($x)           # append (push) to an array (better)
files=(${(f)"$(egrepcmd1l)"} ) # push a sentence to an array (where egrepcmd1l is a global alias
% print ${array:t}
.zshenv .zshrc .zlogout
```

#  Substring extraction ${parameter:start:length} , default length is rest

``` zsh
a=12345
echo ${a:2:2}
34
echo ${a:2}
345
echo ${a[1,3]}
123
```

#  zsh zingers

``` zsh
alias -g NF='*(.om[1])' # newest file
```

# parse xml file putting each tag on a new line

``` zsh
perl -ne 's/(<\/\w+>)/$1\n/g; print' < NF > $(print NF).txt
cat NF > $(print NF).txt
```

#  Variable substitution

Variable | Description
--- | ---
`somevar="bu&^*ck"               `  | variable with mucky characters
`print ${somevar//[^[:alnum:]]/_}`  | replace all non-alphanumerics with _ the // indicates global substitution
`echo ${file##*/}                `  | echo just the file name (strip the path)
`echo ${texfilepath%/*.*}        `  | echo just the path (strip the file name)
`echo ${file%.*}                 `  | strip file extension
`echo $file:r                    `  | strip file extension
`echo ${0##*[!0-9]}              `  | strip all but trailing digit from filename $0
`echo ${(M)0%%<->}               `  | strip all but trailing digit from filename
`file=${1/\//C:\/}               `  | substitute / with c:/ ANYWHERE in string
`file=${1/#\//C:\/}              `  | substitute / with c:/ Beginning of string
`file=${1/%\//C:\/}              `  | substitute / with c:/ End of string
`JUNK=R.E.M.                     `  | substitute last . for a _
`print ${JUNK/.(#e)/_}           `  | substitute last . for a _
`print ${JUNK/%./_}              `  | substitute last . for a _
`wpath=${wpath//\//\\\\}         `  | substitute Unix / with dos \ slashes
`upath=${wpath//\\/\/}           `  | convert backslashes to forward slashes (Dos to Unix
`dpath=${upath/#\/c\//c:/}       `  | convert /c/path/ to c:\path\
`foo=$'bar\n\nbaz\n'             `
`print ${foo//$'\n'}             `  | strip out any carriage returns (some systems use \r)
`print ${foo%%$'\n'}             `  | strip out a trailing carriage return

``` zsh
url='www.some.com/some_strIng-HERe'
anchortext=${${(C)url//[_-]/ }:t}  # titlecase
echo "<a href='$url'>$anchortext</a>"
```

#  Creating a family of functions
## generate hrefs from url

``` zsh
function href{,s} {
  # href creates an HTML hyperlink from a URL
  # hrefs creates an HTML hyperlink from a URL with modified anchor text
  PROGNAME=$(basename $0)
  url=$(cat /dev/clipboard)
  if [ "$PROGNAME" = "href" ]
  then
      href="<a href='$url'>$url"
  elif [ "$PROGNAME" = "hrefs" ]
  then
      anchortext=${${(C)url//[_-]/ }:t}
      href="<a href='$url'>$anchortext"
  fi
  echo -n $col
  echo $href > /dev/clipboard | more
}
```

#  Regular expressions in zsh examples
pcre perl regular expressions

``` zsh
zmodload zsh/pcre
setopt REMATCH_PCRE

var=ddddd; [[ "$var" =` ^d+$ ]] && echo matched || echo did not match
[[ 'cell=456' =` '(cell)=(\d+)' ]] && echo  $match[1,2] $MATCH

var=dddee; regexp="^e+$"; [[ "$var" =` $regexp ]] && echo $regexp matched $var || echo $regexp did not match $var
```

#  Decisions
cd to different drive depending on Windows login name

``` zsh
drive=$([[ "$LOGNAME" != davidr ]] && echo '/o' || echo '/c') # trad way
cd ${drive}/inetpub/wwwdev/www.some.co.uk/
drive=${${${LOGNAME:#davidr}:+/o}:-/c}                        # zsh way
cd ${drive}/inetpub/wwwdev/www.some.co.uk/
```

# Chaining two modifications
.om[1] gives newest file
cyg is a zsh function doing a path conversion e.g. /c/ to C:/

``` zsh
cyg(){reply=("$(cygpath -m $REPLY)")}
gvim $(echo /c/aax/*(.om[1]))(+cyg) &  # nested
gvim /c/aax/*(.om[1]+cyg) &            # both operations
```

# Odd stuff not necessarily zsh

``` zsh
cp -a file1 file   # -a transfer permissions etc of file1 to file2preserve
```
## only copy if destination file exists and is older that source file

``` zsh
[[ -e $L/config.php ]] && cp -p -update $T/config.php $L
```

## variable with variable name

``` zsh
eval "$1=$PWD"
```

## Tips

Tip | Description
--- | ---
setopt autopushd                 | 
dirs -v                          | 
cd `5                            |  cd to fifth directory in directory stack
cd -<tab complete> then type number of directory needs compinit
dirs -p                          |  display recent directories
cp file `1                       |  where 1 is first entry in pushd stack
cp file.txt `+<TAB>           |  select recent directory
ls -1 `1/*(.om[1])               |  newest file previous directory
ls -l  `-/*(.m0)                 |  alternative previous directory `-
pushd +2                         |  cd to 3rd entry in pushd stack

## zsh completion
startfilename<tab>            |  will complete matching files anywhere in $PATH
startfilename<C-D>            |  will list matching files anywhere in $PATH
vi main*`*temp*<tab>          |  avoid file with temp in the name
cd /u/lo/li<tab>   | completes to /usr/local/lib
du -sk *(/) | directory sizes

#  Inline aliases, zsh -g aliases can be anywhere in command line

``` zsh
alias -g G='| grep -'
alias -g L='| less'
#this reduces a command like
ls | grep foo | less
#to
ls G foo L
alias -g R=' > /c/aaa/tee.txt '           # redirect
alias -g T=' | tee /c/aaa/tee.txt '       # tee
alias -g F=' | fmt -'                     # format
alias -g W=' | wc -l'                     # wc
```

#  cd by .. or ... or ... or mv file ..../.

``` zsh
alias '..'='cd ..'
alias -g ...='../..'
alias -g ....='../../..'
alias -g .....='../../../..'
```

#  Suffix based alias

``` zsh
alias -s jpg='/c/program\ files/IrfanView/i_view32.exe'
now just type the image name to launch irfanview
alias -s php='c:/wamp/php/php.exe'  # now just type test.php to execute it
```

# named directories (quick jump to a deep sub-directory)

``` zsh
hash -d zsh="/usr/src/zsh"          # create shortcuts to deep directories
cd `zsh
```

magic equals

``` zsh
vim =some_file                            # edits file anywhere in $PATH
ls =some_file                             # lists file anywhere in $PATH
# magic ** (recursion)
vim **/some_file                          # edits file under under current dir
rm /c/intranet/**/*.stackdump             # specify recursion at a sub-directory
# modifying more than one file (multios)
# writes ls results to file1 & file2 appends to file3
ls > file1 > file2 >> file3 | wc          # multi-io
myscript >&1 >output.txt                  # log a script output
```

Redirection to file as well as send on to pipe:
make install > /tmp/logfile | grep -i error

#  Permissions & ownership

Permission | Description
--- | ---
`ls *(.f644)                  `  |  files with permissions 644
`ls *(.g:root:)               `  |   files belonging to group root
`ls *(.u:apache:)             `  |   files belonging to user apache
`ls -l *(.rwg:nobody:u:root:) `  |    user has read/write permissions
`function g{0..9} { gmark $0 $*  }  ` | declaring multiple functions

#  zmv "programmable rename"
autoload -U zmv

# Replace spaces in filenames with a underline

``` zsh
zmv '* *' '$f:gs/ /_'
zmv '(* *)' '${1// /}'
zmv -Q "(**/)(* *)(D)" "\$1\${2// /_}"
```

#  Change the suffix from *.sh to *.pl

``` zsh
zmv -W '*.sh' '*.pl'
```

#  Lowercase/uppercase all files/directories (-i) interactive

``` zsh
$ zmv -i '(*)' '${(L)1}' # lowercase
$ zmv -i '(*)' '${(U)1}' # uppercase
$ zmv '([a-z])(*).txt' '${(C)1}$2.txt' ; rename fred.txt to Fred.txt
```

#  Initialize zsh/config
autoload -U compinit
compinit

#  case insensitive completion

``` zsh
zstyle ':completion:*' matcher-list 'm:{a-zA-Z}={A-Za-z}' \
     'r:|[._-]=* r:|=*' 'l:|=* r:|=*'
```

#Wonderful zftp (write ftp scripts as though shell)

#  init (could be in .zshenv etc)

``` zsh
autoload -U zfinit
zfinit
zfparams www.someweb.co.uk myuserid mypassword
zfopen
zfcd tips
zfls -l zshtips.html
zfput zshtips.html
zfls -l zshtips.html
```

#  replace every occurence of a file (zsh and bash)

``` zsh
for f in */include/dbcommon.php; do;cp dbcommon.php $f; done
```

alternative for loop
replace every instance of file with new version

``` zsh
for f (**/x) cp newx $f
for f (**/x) {cp newx $f }
for f in **/x; do;cp newx $f; done
```

#  create a clone of a file, modifying it on the fly

``` zsh
for i in {3,4}; sed s/flag=2/flag=$i/ fred.txt > fred$i.txt
for i in {1..9}; sed s/flag=2/flag=$i/ fred.txt > fred$i.txt
```

can be simplified to

``` zsh
for f (*.txt) { echo $f }
for f (*.txt) echo $f   # if no ;
<!-- for f (*(.)) mv $f fixed_$f -->
for f (*.csv.csv) {mv $f ${f:r}} # remove one level of extension
for x ( 1 2 {7..4} a b c {p..n} *.php) {echo $x}
```

#  loop a command

``` zsh
while true; do echo "infinite loop"; sleep 5; done
while (TRUE){echo .;sleep 1}
```

# loop a command

``` zsh
for count in {1..10}; do
r\m x*([1,10]); # delete 10 files at a time
echo "loop $count"
done
for c ({1..50}) {php ./test.php; sleep 5;}
```

#  using vared editor

``` zsh
vared -p "choose 1-3 : " -c ans
case $ans in
 1|a) sdba $key;;
 2|f) sdbf $key;;
 3|i) sdbi $key;;
 *) echo "wrong answer $ans\n" ;;
esac
```

#  select

``` zsh
PROMPT3="Choose File : "
select f in $(ls **/*.tex |egrep -i "${param}[^/]*.tex")
do
 if [[ "$REPLY" = q ]]
 then
    break
 elif [[ -n "$f" ]]; then
    gvim $f
 fi
done
```

#  multiple script commands on same line

``` zsh
if [ $# -gt 0 ];then string=$*;else;string=$(getclip);fi
if grep -iq 'matching' *.php ;then echo "Found" ;else echo "Not Found"; fi
```

#  Command on multilines with a backslash

``` zsh
ls \
> x*
```

#  Command on multilines with a quote

``` zsh
sed '
 s/mvoe/move/g
 s/thier/their/g' myfile
```

#  Editing a variable
vared PATH

Setting | Description
--- | ---
bindkey -v  | vi mode line editting
bindkey -M viins '^O'  | copy-prev-shell-word
bindkey '^L' push-line  | push current command into a buffer, allows you to do another command then returns to previous command

use cat > /dev/null and them press key to get keycode

# configure bindkeys
Setting | Description
--- | ---
bindkey -s '^v<F7>' "ls -l\n" | configure F7 to output 'ls -l'
bindkey -s "^[[18`" "ls -l\n" | You must actually type Control-v F7 at CLI
bindkey -s "^[OS"  "\^d\^c\n" | put a command string onto f4

# bind control-n to scroll most recent file [*N]

``` zsh
zstyle ':completion:most-recent-file:*' match-original both
zstyle ':completion:most-recent-file:*' file-sort modification
zstyle ':completion:most-recent-file:*' file-patterns '*(.)'
zstyle ':completion:most-recent-file:*' hidden all
zstyle ':completion:most-recent-file:*' completer _files
zle -C most-recent-file menu-complete _generic
bindkey "^N"      most-recent-file

zstyle ':completion:expand-args-of::::' completer expand-args-of
bindkey '^x^a' expand-args-of
```

#  Prompt at end of command line

``` zsh
RPROMPT="[%t]" (display the time)
```

#  colo(u)red prompt

``` zsh
fg_light_red=$'%{\e[1;31m%}'
PS3="$fg_light_red Select file : "
```

# print fred in blue color

``` zsh
print '\e[1;34m fred'
```

# color module

``` zsh
autoload colors ; colors
print "$bg[cyan]$fg[blue]Welcome to man zsh-lovers" >> $TTY
PROMPT="%{$bg[cyan]%}%{$fg[red]%}%n%{$reset_color%}%{$bg[cyan]%}@%{$fg[red]%}%m %{$fg[yellow]%}%` %{$reset_color%}%{$bg[cyan]%}%% "
echo "$bg[blue]$fg[yellow]highlight a message"

curl -u userid:password -d status=" updating twitter with from curl " http://twitter.com/statuses/update.xml
```

# .zshenv 
```
autoload -U compinit
compinit
autoload      edit-command-line
zle -N        edit-command-line
bindkey '\ee' edit-command-line
VISUAL='/bin/vim'
EDITOR='/bin/vim'

textify a phrase to create an image name
s="Fred Goat Dog"
print ${(L)s:gs/ /-/}.jpg
or
print ${(L)s// /-}.jpg

and to de-textify an image

foo=fred-goat-dog.jpg
echo ${(C)foo:gs/-/ /:r}
or
print ${${(Cs:-:):-fred-goat-dog.jpg}%.*}

#new
#" read a file into a variable
var="$(<file)"
```
# zsh syntax synopis
| Command                                                  | Description                                                      |
|---------------------------------------------------------- | ------------------------------------------------------------------|
| `cd -<TAB>`                                              | list recent dirs                                                 |
| `cp 1.php -<TAB>`                                        | list recent dirs                                                 |
| `vi *(om[1])<TAB>`                                       | edit newest file                                                 |
| `ls *(.mh3)`                                             | modified 3 hours ago                                             |
| `ls *(.mh-3)`                                            | less than 3 hours                                                |
| `ls *(.mh+3)`                                            | more than 3 hours                                                |
| `gvim -p *(m0)`                                          | all files modified today                                         |
| `mv *.*(^m-1) old/`                                      | move all but todays files to sub-directory                       |
| `vi -p *(.om[1,3])`                                      | open 3 newest files in tabs (gvim)                               |
| `ls *(^m0)`                                              | files NOT modified today                                         |
| `ls -l *(m4)`                                            | list files modified exactly 4 days ago                           |
| ```ls `1/*(.om[1])```                                        | list newest file from previous directory needs setopt autopushcd |
| ```ls -lt  **/*`pdf/*(.m0om[1,10])```                        | up to 10 files modified in last 24 hrs                           |
| `for f (*.php) { diff $f  ${PWD/html/staging}/$f}`       | diff all files in parallel directories                           |
| `mv !?main<TAB>!=`                                       | recall previous command containing the string main               |
| `mv !?main?$<tab>`                                       | recall just last parameter of command containing the string main |
| `scp -rp *(om[1]) root@192.168.168.157:$PWD`             |                                                                  |
| `scp -rp *(om[1]) root@192.168.168.157:${PWD/test/live}` | modify pwd remote path                                           |
| `!?client`                                               | recall most recent cmd containing string 'client'                |
| `vi !?client?$<tab>`                                     | recall just last parameter of cmd containing 'client'            |
| ` ls (x*`x[3-5])`                                         | list files x* except x3 to x5                                    |
| `ls \*\*/*`*/.git/*`                                       | ignore all git subdirectories *`* matches a path                 |
| `!!:gs/fred/joe/`                                        | edit previous command replace all fred by joe                    |
| `cp NF ${PWD/html/release}`                              |                                                                  |
| `while (TRUE){echo -n .;sleep 1}`                        |                                                                  |
| `ls (^(backup*|cache*|list*|tmp)/)|#*(.)` | ignore 4 directories  |
| `ls *.(jpg|gif|png)(.)` | do not break if one or other image type absent |

# Examplars
```
zargs **/*.(js|php|css)`(djr|libs|dompdf)/*`*/junk/* -- grep -i
alias phpall='for f (*.php`test.php(om)) {php -l $f} | more'
alias phpsub=' for f (./(async|inc)/*.php`test.php(om) *.php(om)) {php -l $f} | more'
alias diffall='for f (*.php`test.php(.om)){diff -q $f ${PWD/html/staging}/$f} 2>&1 | grep differ'
alias -g STAG=' ${PWD/html/staging}'
mv Licence\ to\ Print\ Money.pdf !#^:gs/\\ //
A=(1 2 5 6 7 9) # pre-populate an array
for ((i=1;$#A[i];i++)) echo $A[$i]
```


# Debugging
Enable XTRACE if you want to echo whats happening in your script
```
setopt XTRACE VERBOSE
unsetopt XTRACE VERBOSE
```

# misc
```
bindkey "^[t" transpose-words # switch two previously typed words with alt-t
setopt interactivecomments  # allows end of command line comments
take(){[ $# -eq 1 ]  && mkdir "$1" && cd "$1"} # create a directory and move to it in one go
zmodload -F zsh/stat b:zstat
zstat -g canal-bridge.mov # full files details
# remember current directory : double quotes vital
alias markcd="cd $(pwd)"
```

# run a remote zsh script via ssh
```
ssh 192.168.1.218 -l root "zsh -c 'for i (/usr/*(/)) {ls \$i }'"
# compare local & remote file size
FILE=$(echo *(.om[1])) && ls -l $FILE && ssh 192.168.1.1 -l root "zsh -c 'ls -l $PWD/$FILE'"
# remote login in with zsh
ssh -t root@192.18.001.001 'sh -c "cd /tmp && exec zsh -l"'
# zsh menu
echo "enter 0-2,a"
read ans ; # read in a parameter
case "$ans" in
    0|${prog}0) cd "$(cat /c/aam/${prog}0)" ;;
    1|${prog}1) cd "$(cat /c/aam/${prog}1)" ;;
    2|${prog}9) cd "$(cat /c/aam/${prog}9)" ;;
    a|${prog}l) cd "$(cat /c/aam/${prog}a)" ;;
    **) echo "wrong number $ans\n" ;;
esac
```

# Useful aliases
| Alias                                                                              | Description                                                           |
| `alias flip='cd html live > /dev/null &>1  cd live html > /dev/null &>1' | swap ` | flip between 2 deep parallel directories |
| `alias phpall='for f (*.php test.php(om)) {php -l $f}'`                            | syntax-check all php files in date order excluding some               |
| `alias diffall='for f (*.php test.php(.om)) { diff -q $f  ${PWD/html/staging}/$f}'` | diff all files in parallel directories in date oder (excluding some ) |

# locating/identifying things
```
which zsh
whence -vsa ${(k)commands[(I)zsh*]}  # search for zsh*
locate zsh
cd $(locate -l1 -r "/zoo.txt$")(:h) # cd to directory of first occurence of a file zoo.txt (N)
cd ${$(locate zoo.txt)[1]:h}
cd **/resource.php(:h)
```
