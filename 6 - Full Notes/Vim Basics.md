2026-10-03 15:43

Tags: [[shell]]

# Vim Basics
- Vim separates movement, insertion, selection, and commands into modes.
- Return to normal mode with `Esc` before composing navigation or editing commands.
- Learn a small reliable vocabulary first rather than memorising every shortcut.

## Essential Modes
| Mode | Enter or use |
| --- | --- |
| Normal | Default; press `Esc` to return |
| Insert | `i` inserts text before the cursor |
| Visual | `v` begins character selection |
| Command-line | `:` starts an editor command |

## Movement
- `h`: left; `j`: down; `k`: up; `l`: right.
- `w`: next word; `b`: previous word.
- A count repeats an action: `3j` moves down three lines.
- Prefer motions over repeated individual arrow presses as they become familiar.

## Editing and Recovery
- `dd`: delete the current line.
- `u`: undo.
- `Ctrl+r`: redo.
- In visual mode, select text and press `y` to yank it.
- In normal mode, `p` puts copied or deleted text after the cursor; linewise text goes below the current line.

## Save and Exit
- `:w`: write the file.
- `:q`: quit when there are no unsaved changes.
- `:wq`: write and quit.
- `:q!`: quit and discard unsaved changes; use deliberately. [Vim first steps](https://vimhelp.org/usr_02.txt.html).

## Practice Loop
1. Open a disposable practice file.
2. Insert several lines, then press `Esc`.
3. Navigate with word motions and counts.
4. Delete a line, undo, and redo.
5. Select, yank, and put text.
6. Save and exit.

The main habit is knowing the current mode, not typing faster.

# References
[[2 - Source Materials/Videos/Youtube Videos/Vim As Your Editor - Introduction|Vim As Your Editor - Introduction]]

