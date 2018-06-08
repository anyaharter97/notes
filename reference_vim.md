# vim

Many hotkeys let you enter a number _&lt;n&gt;_ before the command to run that command _&lt;n&gt;_ times.

## Configuration
`~/.vimrc` holds a lot of the configurations of vim   
`~/.vim/colors` holds all colors defined for vim  

## Toggling Modes
| HotKey | Action                                                       |
| ------ | ------------------------------------------------------------ |
| `i`    | toggles insert mode                                          |
| `v`    | toggles visual mode (all movement results in selecting text) |
| `esc`  | exits insert or visual mode                                  |
| `:`    | toggles command mode                                         |

## General
| HotKey      | Action                  |
| ----------- | ----------------------- |
| `.`         | repeat previous command |
| `u`         | undo                    |
| `ctr` + `r` | redo                    |
| `:help`     | help                    |
| `:w`        | save                    |
| `:q`        | quit                    |
| `:q!`       | quit without saving     |

## Movement
| HotKey           | Action                    |
| ---------------- | ------------------------- |
| **By Character** |                           |
| `h`              | left                      |
| `k`              | up                        |
| `l`              | right                     |
| `j`              | down                      |
| **By Word**      |                           |
| `w`              | start of next word        |
| `e`              | end of current word       |
| `b`              | beginning of current word |
| `ge`             | end of previous word      |
| **By Line**      |                           |
| `0`              | beginning of line         |
| `$`              | end of line               |
| **By More**      |                           |
| `gg`             | beginning of file         |
| `G`              | end of file               |
| _n_ `G`          | jumps to line _&lt;n&gt;_ |

## Searching
| HotKey           | Action                                             |
| ---------------- | -------------------------------------------------- |
| **By Character** |                                                    |
| `f` _c_          | finds next occurrence of character _&lt;c&gt;_     |
| `F` _c_          | finds previous occurrence of character _&lt;c&gt;_ |
| **By Word**      |                                                    |
| `*`              | find next occurrence of word under cursor          |
| `#`              | find previous occurrence of word under cursor      |
| **By More**      |                                                    |
| `/<text>`        | finds next occurence of _&lt;text&gt;_             |
| `n`              | forward search on _&lt;text&gt;_                   |
| `N`              | backward search on _&lt;text&gt;_                  |
| **Brackets**     |                                                    |
| `%`              | find the corresponding end bracket                 |
| `shift` + `%`    | find the corresponding start bracket               |

## Insertion, Deletion, and Replacement
| HotKey             | Action                                                         |
| ------------------ | -------------------------------------------------------------- |
| **By Character**   |                                                                |
| `x`                | delete character under the cursor                              |
| `X`                | delete character to the left of the cursor                     |
| `r`                | replace character under cursor                                 |
| **By Line**        |                                                                |
| `o`                | insert newline below                                           |
| `O`                | insert newline above                                           |
| `dd`               | delete line                                                    |
| _n_ `dd`           | delete _&lt;n&gt;_ lines                                       |
| `S`                | clear current line                                             |
| **By More**        |                                                                |
| `d` _m_            | delete all text covered by movement _&lt;m&gt;_                |
| **Copy and Paste** |                                                                |
| `y`                | "yank" copy selected text                                      |
| `yy`               | "yank" copy line                                               |
| `p`                | paste copied text (this also pastes text deleted by d command) |
