# Lecture 3 - Vim

## Modal Editing

Vim has multiple modes: normal, insert, replace, visual, command.

## Normal Mode

| Action | Command |
| :--- | :--- |
| word forward, word backwards | w, b |
| end of word | e |
| begin line, end line, first non-empty | 0, $, ^ |
| scroll upward, scroll downard \(buffer\) | ^U, ^D |
| start of buffer, end of buffer | gg, G |
| change cursor to {highest, middle, lowest} line | H, M, L |
| find next character \(within line\), backwards find | f{char}, F{char} |
|  |  |

## Command Mode

You can enter the command mode by entering a colon `:` .

| Action | Command |
| :--- | :--- |
| close window, quit editor | :q, :quit |
| close all windows,, quit editor | :qa |
| save file | :w |
| get help | :h. :help  |
| new tab | :tabnew |
| split view | :sp |

