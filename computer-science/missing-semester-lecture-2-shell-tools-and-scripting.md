# Missing Semester - Lecture 2 - Shell Tools and Scripting

## Full Lecture

```bash
# Variable definition
foo=bar

# Be aware of spaces in between. Something like
# bash> foo = bar
# would actually call "foo" with "=" and "bar" as positional arguments

# Strings
echo "Hello" # strings with double-quotes
echo 'Hello' # literal, strings with single-quotes
echo "Foo value is $foo" # prints "Foo value is bar"
echo 'Foo value is $foo' # prints "Foo value is $foo"

# Define functions
# on a new .sh file
mcd () {
    mkdir -p "$1"
    cd "$1"
}

## Dollar Numbers - Reserved Keywords
# $0 will be the name of the script
# $1 to $9 : 1st to 9th 
# $? error code of previous command. 0 means everything went fine, no errors.
# $_ last argument of previous comment
# $# gets number of arguments
# $$ gets the pid
# $@ expands to all the arguments.

# Booleans 
## Booleans and Errors
true
echo $? # 0
false
echo $? # 1

## Booleans and Logical
# Logical Priors and Conditionals

# Executes the first one. If sucessful, executes the second one
# If the first didn't had a 0 error code (i.e. all good, a.k.a. true), 
# then execute the second.
false || echo "oops failed" 
true || echo "won't be printed" # logical OR only requires one true for true.

# Sleep on Booleans
true && sleep 5 # Waits
false && sleep 5 # Gets executed immediately. Second condition is not ran.

# Command concatenation using semmicolon
false ; echo "always prints"

# Test utility allows to exit with a status determined by an expression.
# -eq, -ge, -lt : equal, greater or equal than, less than (two Ints)
# -e, -f : file exists, file exists and is regular file
# -d : file exists and is directory
# further info on `man test`

# Curly Braces
echo foo{,1,2,3}   # expands to "foo foo1 foo2 foo3"
echo f{A,B}_d{C,D} # fA_dC, fA_dD, fB_dC, fB_dD
echo file{1..3}    # file1 file2 file3

# Returns the difference files between 2 folders
diff <(ls folderA) <(ls folderB)


# Shebang a.k.a #!    
# We can add the executable path to the beginning of the bash file so that
# it knows how to be executable.
# This alternative uses the env 
#!/usr/bin/env python

# shellcheck allows to have deeper insights into your .sh script
# shellcheck my-awesome-script.sh

# Helper Methods
# man is an interface for system reference manuals
# tldr is a simplified, community-driven alternative

# Finding Stuff
# find not only allows to find files and directories in the dir tree 
# but also to execute commands
find . -name "*.csv" -exec cat {} + # prints all the contents under CSVs.

# grep allows searching within the files' contents
grep foo mcd.sh # prints lines with foo in mcd.sh file
grep -R foo .   # recursive search within directory

# ripgrep a.k.a rg - recursive line-oriented CLI search tool
rg mahalanobis # searches for mahalanobis pattern within directory

# fzf - fuzzy finder
history > fzf

# List Directory
tree # prints the directory tree
```

## Example \#1 

```bash
#!/bin/bash

echo "Starting program at $(date)"
echo "Running program $0 with $# with the process id $$"

for file in "$@"; do
    grep foobar "$file" > /dev/null 2> /dev/null
    # when pattern is not found, grep has exit status 1 (i.e. error)
    # redirect stdout and stderr to a null register
    
    if [[ "$?" -ne 0 ]]; then # if last exit status is not success
        echo "File $file does not have any foobar, adding one"
        echo "# foobar" >> "$file" # appends string
    fi
end
    
    
    # /dev/null is a special feature on UNIX systems where you can write
    # and it will be dicarded
    
    # Redirection commands
    # > for stdout, 2> for stderr, >> for append
    
```

## Bang Comments

```bash
Syntax
      !!       Run the last command again
      !foo     Run the most recent command that starts with 'foo' (e.g. !ls)
      !foo:p   Print out the command that !foo would run
               also add it to the command history
      !$       Run the last word of the previous command (same as Alt + .)
      !$:p     Print out the word that !$ would substitute
      !*       Run the previous command except for the last word
      !*:p     Print out the previous command except for the last word
     ^foo^bar  Run the previous command replacing foo with bar
```

