# vim
* [Configuration](vim.md#configuration)
* [Hot Keys](vim.md#hot-keys)
    * [Toggling Modes](vim.md#toggling-modes)
    * [General](vim.md#general)
    * [Movement](vim.md#movement)
    * [Searching](vim.md#searching)
    * [Deleting, Inserting, Replacing, and Copy/Pasting](vim.md#deleting-inserting-replacing-and-copy-pasting)
* [Find and Replace](vim.md#find-and-replace)
    * [\[range\]](vim.md#range)
    * [{pattern}](vim.md#pattern)
    * [{string}](vim.md#string)
    * [\[flags\]](vim.md#flags)
    * [Examples](vim.md#examples)

## Configuration
`~/.vimrc` holds a lot of the configurations of vim   
`~/.vim/colors` holds all colors defined for vim  

## Hot Keys

Many hotkeys let you enter a number _&lt;n&gt;_ before the command to run that command _&lt;n&gt;_ times.

#### Toggling Modes
| key   | action                                     |
| ----- | ------------------------------------------ |
| `i`   | toggles insert mode                        |
| `v`   | toggles visual mode (movement = selection) |
| `esc` | exits insert or visual mode                |
| `:`   | toggles command mode                       |

#### General
| key         | action                  |
| ----------- | ----------------------- |
| `.`         | repeat previous command |
| `u`         | undo                    |
| `ctr` + `r` | redo                    |
| `:help`     | help                    |
| `:w`        | save                    |
| `:q`        | quit                    |
| `:q!`       | quit without saving     |

#### Movement
| key     | action                    |
| ------- | ------------------------- |
| `h`     | left                      |
| `k`     | up                        |
| `l`     | right                     |
| `j`     | down                      |
| `w`     | start of next word        |
| `e`     | end of current word       |
| `b`     | beginning of current word |
| `ge`    | end of previous word      |
| `0`     | beginning of line         |
| `$`     | end of line               |
| `gg`    | beginning of file         |
| `G`     | end of file               |
| _n_ `G` | jumps to line _&lt;n&gt;_ |

#### Searching
| key       | action                                             |
| --------- | -------------------------------------------------- |
| `f` _c_   | finds next occurrence of character _&lt;c&gt;_     |
| `F` _c_   | finds previous occurrence of character _&lt;c&gt;_ |
| `*`       | find next occurrence of word under cursor          |
| `#`       | find previous occurrence of word under cursor      |
| `/<text>` | finds next occurence of _&lt;text&gt;_             |
| `n`       | forward search on _&lt;text&gt;_                   |
| `N`       | backward search on _&lt;text&gt;_                  |
| `%`       | find the corresponding end bracket                 |

#### Deleting, Inserting, Replacing, and Copy/Pasting
| key                | action                                          |
| ------------------ | ----------------------------------------------- |
| **Deletion**       |                                                 |
| `x`                | delete character under the cursor               |
| `X`                | delete character to the left of the cursor      |
| `dd`               | delete line                                     |
| _n_ `dd`           | delete _&lt;n&gt;_ lines                        |
| `S`                | clear current line                              |
| `d` _m_            | delete all text covered by movement _&lt;m&gt;_ |
| **Replacement**    |                                                 |
| `r`                | replace character under cursor                  |
| **Insertion**      |                                                 |
| `o`                | insert newline below                            |
| `O`                | insert newline above                            |
| **Copy and Paste** |                                                 |
| `y`                | "yank" copy selected text                       |
| `yy`               | "yank" copy line                                |
| `p`                | paste copied (or `d`eleted) text                |


## Find and Replace

The basic find and replace command has several different parts:
```
:[range]s/{pattern}/{string}/[flags]
```

### [range]
`%` perform substitution on whole file  
`1,10` only do substitution for lines 1-10  
`.,$` current to last line  
`.,+2` current line and next two lines  
`g/^baz/` each line starting with 'baz'  

### {pattern}
`{text}` simple recognition of any string matching `{text}`  
`\<{text}\>` forces substitution to search only for the full word (`\<` and `\>` are word boundary characters)  
`\({text1}\|{text2}\)` substitutes either text (`\|` is "logical or")  

> **Details**
>
>`.`, `*`, `\`, `[`, `^`, and `$` are metacharacters
>
>`+`, `?`, `|`, `&`, `{`, `(`, and `)` must be escaped to use their special function
>  
>`\/` is / (use backslash + forward slash to search for forward slash)
>
>`\t` is tab, `\s` is whitespace (space or tab)  
>`\n` is newline, `\r` is CR (carriage return = Ctrl-M = ^M)
>
>After an opening `[`, everything until the next closing `]` specifies a **collection**. Character ranges can be represented with a `-`; for example a letter a, b, c, or the number 1 can be matched with `[1a-c]`. Negate the collection with `[^` instead of `[`; for example `[^1a-c]` matches any character except a, b, c, or 1.
>
>`\{#\}` is used for repetition. `/foo.\{2\}` will match foo and the two following characters.
>
>`\(foo\)` makes a backreference to foo. Parenthesis without escapes are literally matched.

### {string}
`{text}` simple substitution of the string `{text}`  

>**Details**
>
>`\r` is newline, `\n` is a null byte (0x00).
>
>`\&` is ampersand (& is the text that matches the search pattern).
>
>`\0` inserts the text matched by the entire pattern
>
>`\1` inserts the text of the first backreference. `\2` inserts the second backreference, and so on.


### [flags]
`c` confirm each substitution  
`g` replace all occurrences in the line  
`i` ignore case for the pattern  
`I` make pattern case-sensitive  

With the confirm flag, vim will highlight a word and give options (y/n/a/l/^E/^Y):  
* `y` replace current highlighted word  
* `n` do not replace current highlighted word  
* `a` replace this and all remaining matches  
* `l` replace this and quit  
* `^E` scroll up  
* `^Y` scroll down  

### Examples
`:%s/\<his\>/her/`

`:%s/\(good\|nice\)/awesome/g`

`:%s/\<\(hey\|hi\)\>/hai/g`

Save typing by using `\zs` and `\ze` to set the start and end of a pattern. For example:  
`:%s/Copyright \zs2007\ze All Rights Reserved/2008/`

`:%s/I \(.*\) to eat \(.*\)\./I \1 to eat \2!/g`

`%s/if (!virtDBusConnectOpen(connect, \(.*\)))/if (!virtDBusConnectOpen(connect, \1))/g`
