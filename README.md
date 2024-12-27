The [Zettelkasten](#Zettelkasten-Embracing-Tags-Over-Folders) note-taking method gathers all assorted notes, *Zettels* in german, in a single folder, *Kasten*, as crawlable text (or markdown) files that can be interlinked by name.

`Zappykasten` is a Vim plug-in that lets you

- crawl note folders (thanks to [fzf](https://github.com/junegunn/fzf) and [ripgrep](https://github.com/BurntSushi/ripgrep))
- create a note with the designated search terms if it is yet absent,
- insert a link to a note through content searches, and jump to/preview linked notes
- insert and complete tags, and search for them

## Requirements

`Zappykasten` should work on Linux, Mac and Microsoft Windows, notably in Git Bash and WSL.
Ensure you have Vim installed along with [Ripgrep](https://github.com/BurntSushi/ripgrep) and [FZF(.vim)](https://github.com/junegunn/fzf.vim);
these tools are necessary for the plugin to function correctly.
To install `ripgrep`, either use your package manager or download a binary from his release page [Ripgrep](https://github.com/BurntSushi/ripgrep/releases) and place it in your `$PATH`.

## Installation

If you use [vim-plug](https://github.com/junegunn/vim-plug), then in the vim-plug block in your `.vimrc` file, add the following line between the `plug#begin` and `plug#end` calls:

```vim
Plug 'https://github.com/junegunn/fzf', { 'dir': $HOME..'/bin/apps/fzf' }
    Plug 'junegunn/fzf.vim'
        Plug 'Konfekt/zappykasten.vim'
```

## Initial Setup

Set the directories in which to search for notes and a *main* directory, in which to store new notes (which both default to `~/zettelkasten`):

```vim
let g:zk_search_paths = [$HOME..'/zettelkasten']
let g:zk_main_directory = g:zk_search_paths[0]
" let g:zk_search_paths += [$HOME..'/diary']
" let g:zk_search_paths += [$HOME..'/Desktop']
```

If you come from [Alok Singh's notational-fzf-vim](https://github.com/alok/notational-fzf-vim), then it should be a matter of simply replacing all occurrences of initial `nv` in the variable names by `zk`.
On Microsoft Windows, all paths are assumed to be either on mounted drives or UNC sharepoints depending on `g:zk_main_directory`.

## Instructions

To use `Zappykasten`:

- In Normal mode, type `:ZK` to search for notes by content.
    If a note doesn't exist, it will be created with a title derived from your search terms.
    (Use a mapping, say `nnoremap m, :<c-u>ZK<cr>` for faster access.)
- In Normal mode, press
    - `gf`/`K`  to jump/preview the linked note under the cursor,
    - `[<C-D>` to jump to the linked tag under the cursor,
    - `[D/I` to list all occurrences of the (tagged) keyword under the cursor (see `:help include-search`).
- In insert mode:
    - press `<C-X><C-I>` to complete words from other notes.
    - press `<C-X><C-Z>` to insert a link to a note (Zettel), searching for the term before the cursor, which can be refined in a fuzzy searcher.
    - to insert a tag, such as `Zettel`, surround it with double brackets (such as in `[[Zettel]]`).

## Key Bindings in Fuzzy Search Window

Once invoked the Fuzzy Search Window by `:ZK`, you can hit

- `Ctrl-X`: Create a new note.
- `Ctrl-Y`: Yank the selected filenames.
- `Ctrl-S`: Split the current window.
- `Ctrl-V`: Create a vertical split.
- `Ctrl-T`: Open a new tab with the note.

## Configuration

- Default Extension: Specifies the default file extension for new notes.
```
let g:zk_default_extension = '.md'
```

- Tag Marker: Specifies the markers for tags; defaults to surrounding double brackets `[[...]]`
```
let g:zk_tag_marker  = '\[\[\ze\i\+\]\]'
```

- Insert Link to Note Key: Defines the key binding to insert a link to a note.
```
let g:zk_insert_note_key = '<c-x><c-z>'
```

- Insert Link to Note Path: can be

    - 'relative' : insert the path relative to the directory of the current file
    - 'absolute' : insert the absolute path of the file (with a tilde for the home directory)
    - 'name' : insert only the name of the file

    ```vim
    let g:zk_insert_note_path = 'name'
    ```

- Create Note Key: Defines the key binding to create a new note.
```
let g:zk_create_note_key = 'ctrl-x'
```

- Yank Key: Defines the key binding to yank selected file names.
```
let g:zk_yank_key = 'ctrl-y'
```

- Keymap: Custom key bindings for various note management actions.
```
let g:zk_keymap = {
    \ 'ctrl-s': 'split ',
    \ 'ctrl-v': 'vertical split ',
    \ 'ctrl-t': 'tabedit ',
    \ }
```

- Include Hidden Files: Includes hidden files and folders in searches.
```
let g:zk_include_hidden = 0
```

- Use Ignore Files: Respects ignore files (like .gitignore) in search paths.
```
let g:zk_use_ignore_files = 1
```

- Ignore Patterns: additional ignore patterns.
```
let g:zk_ignore_pattern = ''
```

- Use Short Path names: Truncates each path element to a single character.
```
let g:zk_use_short_pathnames = 1
```

- Window Width: Sets the width of the note window as a percentage of the screen's width.
    By default it toggles between `40%` and `60%` depending on the number of screen lines.
```
let g:zk_window_width = '40%'
```

- Create Note Window: Controls how the new note window is created.
```
let g:zk_create_note_window = 'vertical split'
```

- Window Direction: Determines the placement of the note window.
```
let g:zk_window_direction = 'down'
```

- Window Command: Specifies the command to open the note window.
```
let g:zk_window_command = 'call my_function()'
```

- Show Preview: Enables a preview window during searches.
```
let g:zk_show_preview = 1
```

- Wrap Preview Text: Enables text wrapping in the preview window.
```
let g:zk_wrap_preview_text = 1
```

- Preview Width: Sets the width of the preview window as a percentage of the screen's width.
```
let g:zk_preview_width = 50
```

- Preview Direction: Determines the placement of the preview window.
    By default it toggles between `right` and `down` depending on the number of screen columns.
```
let g:zk_preview_direction = 'right'
```

- Yank Separator: Specifies the separator used between yanked file names.
```
let g:zk_yank_separator = "\n"
```

### Ripgrep Options

```vim
let g:zk_rg_options = [
      \ '--follow',
      \ '--smart-case',
      \ '--line-number',
      \ '--color never',
      \ ])
```

This variable defines the default options used with Ripgrep when invoked within the context of this application.
The options set here influence how Ripgrep searches through files.
Here are the options set by default:

- `--follow`: Instructs Ripgrep to follow symbolic links during the search.
- `--smart-case`: Enables case-insensitive searching unless the search query contains uppercase characters, which triggers a case-sensitive search.
- `--line-number`: Includes line numbers in the Ripgrep output, which can be useful for reference.
- `--color never`: Disables colored output, ensuring compatibility with interfaces that do not support ANSI colors.

These options are stored in a Vim script list and can be customized by setting the `zk_rg_options` global variable.

### FZF Options

```vim
let g:zk_fzf_options = [
      \ '--ansi',
      \ '--multi',
      \ '--exact',
      \ '--inline-info',
      \ '--tiebreak='..'length,begin' ,
      \ '--preview='..shellescape('rg --pretty --context 3 -- {q} {1}'),
      \ ])
```

This variable configures the default behavior of FZF, a command-line fuzzy finder, within the application.
The settings specified here affect how results are displayed and interacted with in FZF.
The default configuration includes:

- `--ansi`: Allows ANSI color codes in the output, which is useful for colored display of results.
- `--multi`: Enables the ability to select multiple items in the FZF interface.
- `--exact`: Turns on exact-match mode, which restricts matches to exactly what is typed, as opposed to fuzzy matching.
- `--inline-info`: Displays search information (such as the number of matches) inline, rather than as a separate line.
- `--tiebreak=length,begin`: Sets the criteria for resolving items that score the same during the search. It prioritizes shorter items and those that match near the beginning.
- `--preview`: Uses Ripgrep to generate a preview of each item.

# Zettelkasten: Embracing Tags Over Folders

A [Zettelkasten](https://zettelkasten.de/) consists of assorted text files, or **zettels**, which may link to each other by name.
Traditionally, these were stored in a card file box; today, they are kept in a digital directory, often in Markdown format.

Folders, whether paper-based or digital, traditionally organize files.
However, since our computers (with tools like `(rip)grep`) search across the contents of thousands of files in milliseconds, a folder tree to organize files is no longer required:

If we think of the folder as a category tag, then a folder attaches a single tag to a file;
yet, a file often pertains to more than one category.
Tagging, which integrates keywords directly into the text, is by any means much more practical than managing multiple folders;
even more so since these tags usually need not be explicitly added, but already appear in the text.
(Similar to finding a browser bookmark by its title instead of folder navigation.)

Best, this method aligns with the workings of our mind, as we often recall files by remembering related words rather than a single label.

# Credits

This is a fork of [Alok Singh's notational-fzf-vim](https://github.com/alok/notational-fzf-vim) to whom all credit shall be due and whose license restrictions apply.
If stands on the shoulders on Vim along with `ripgrep` and `fzf`.
