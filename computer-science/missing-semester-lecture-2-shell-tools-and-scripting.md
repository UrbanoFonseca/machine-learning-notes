# Missing Semester - Lecture 2 - Shell Tools and Scripting

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
# $? error code of previous command
# $_ last argument of previous comment
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

