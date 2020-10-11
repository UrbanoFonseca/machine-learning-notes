# Lecture 4 - Data Wrangling

## Running on Server Side

`less` is a **pager** \(just like man\) which is basically a way to fit a large content into the terminal window.

If you are grepping stuff from  a remote large file, the network can be a main bottleneck and you should think about running some filters on the server and just outputting what you need afterwards.

```bash
# Alt 1 - Greps locally
ssh {..} | grep something | grep else | less 
# Alt 2 - Everything heavy on server, view results to less
ssh {..} ' grep something | grep else' | less 
```

## Stream Editing

`sed` is a stream editor that allows to make changes to the contents of a stream. 

```bash
# Structure of sed substitute (s) command
# s/searchstring/replacestring

# Examples
sed 's/.*timestamp//' # removes everything in line before timestamp
```

## Regex

```bash
# Regex
# . any given character
# * (prev) 0 or more
# + (prev) 1 or more
# ? (prev) 0 or 1. may or may not occur. 
# [] matches any character within set
# [^] mstches any character not within set

# By default, regex applies first match
# override behaviour with g (global) command

# Begin, End Line
# ^ Special character that matches the beginning of a line
# $ Special character that matches the end of a line

# Capturing Groups
# Allows to remember the values and use them later.
# In the replacement, indicate the index with \ (e.g. \2 prints content of 2nd group)
# () Groups multiple tokens together, capture group for extracting a substring or using a backreference.

```

## Awk

`awk` is a column based stream processor.

