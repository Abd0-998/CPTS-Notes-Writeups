## Modes 

| command  | description                 |
| -------- | --------------------------- |
| `i`      | Insert mode (before cursor) |
| `I`      | Insert at line beginning    |
| `a`      | Insert mode (after cursor)  |
| `A`      | Insert at line end          |
| `o`      | New line below              |
| `O`      | New line above              |
| `Esc`    | Return to Normal mode       |
| `v`      | Visual mode                 |
| `V`      | Visual line mode            |
| `Ctrl+v` | Visual block mode           |

## Save and Quit 

| command                                                                | Description         |
| ---------------------------------------------------------------------- | ------------------- |
| [`:w`](https://linuxize.com/post/how-to-save-file-in-vim-quit-editor/) | Save file           |
| `:w filename`                                                          | Save as filename    |
| `:q`                                                                   | Quit                |
| `:q!`                                                                  | Quit without saving |
| `:wq`                                                                  | Save and quit       |
| `:x`                                                                   | Save and quit       |
| `ZZ`                                                                   | Save and quit       |
| `ZQ`                                                                   | Quit without saving |

## Navigation 

| command | Description          |
| ------- | -------------------- |
| `h`     | Move left            |
| `j`     | Move down            |
| `k`     | Move up              |
| `l`     | Move right           |
| `w`     | Next word            |
| `b`     | Previous word        |
| `e`     | End of word          |
| `0`     | Line beginning       |
| `$`     | Line end             |
| `^`     | First non-blank char |
## Jump around 
| command  | Description              |
| -------- | ------------------------ |
| `gg`     | Go to first line         |
| `G`      | Go to last line          |
| `5G`     | Go to line 5             |
| `:5`     | Go to line 5             |
| `Ctrl+f` | Page down                |
| `Ctrl+b` | Page up                  |
| `Ctrl+d` | Half page down           |
| `Ctrl+u` | Half page up             |
| `%`      | Jump to matching bracket |
| `H`      | Top of screen            |
| `M`      | Middle of screen         |
| `L`      | Bottom of screen         |
## Copy & Paste 
| command                                                         | Description                |
| --------------------------------------------------------------- | -------------------------- |
| [`yy`](https://linuxize.com/post/how-to-copy-cut-paste-in-vim/) | (copy) line                |
| `yw`                                                            | copy word                  |
| `y$`                                                            | copy to line end           |
| `5yy`                                                           | copy 5 lines               |
| [`ggVG`](https://linuxize.com/post/vim-select-all/)             | Select all lines           |
| `p`                                                             | Paste after cursor         |
| `P`                                                             | Paste before cursor        |
| `"*y`                                                           | copy to system clipboard   |
| `"*p`                                                           | Paste from clipboard       |
| `"+y`                                                           | copy to clipboard (X11)    |
| `"+p`                                                           | Paste from clipboard (X11) |
|                                                                 |                            |

## Delete
| command                                            | description               |
| -------------------------------------------------- | ------------------------- |
| `x`                                                | Delete character          |
| `X`                                                | Delete char before cursor |
| [`dd`](https://linuxize.com/post/vim-delete-line/) | Delete line               |
| `D`                                                | Delete to line end        |
| `dw`                                               | Delete word               |
| `d$`                                               | Delete to line end        |
| `d0`                                               | Delete to line start      |
| `dG`                                               | Delete to file end        |
| `dgg`                                              | Delete to file start      |
| `5dd`                                              | Delete 5 lines            |

## Undo & Redo 
| command                                         | Description         |
| ----------------------------------------------- | ------------------- |
| [`u`](https://linuxize.com/post/vim-undo-redo/) | Undo                |
| `U`                                             | Undo line changes   |
| `Ctrl+r`                                        | Redo                |
| `.`                                             | Repeat last command |