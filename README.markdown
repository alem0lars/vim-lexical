# vim-lexical

> Building on Vim’s spell-check and thesaurus/dictionary completion

Features of this plugin:

* Specify the languages to be used in spell-check
* Specify a list of thesauruses for synonym completion
* Specify a list of dictionaries for word completion
* Opt-in key mappings for _Normal_ mode thesaurus and dictionary
  completion
* Buffer-scoped configuration (leaves your global settings alone)

Though principally used as a editor for code, Vim flirts with those of us
editing documentation and prose by providing spell-check as well as
completion capabilities using both dictionary and thesaurus files.

While we can configure these settings in our `.vimrc` files, we often need
more granular control, where defaults are leveraged and configuration is
applied by file type to the current buffer. This plugin fills that gap.

## Installation

Best Installed using Pathogen, Vundle, Neobundle, or your favorite Vim
package manager.

## Configuration

Because spell-check, thesaurus, etc. isn’t needed for all file types, you can 
configure it per file type in your `.vimrc`:

```vim
augroup lexical
  autocmd!
  autocmd FileType markdown call lexical#init()
  autocmd FileType textile call lexical#init()
  autocmd FileType text call lexical#init({ 'spell': 0 })
augroup END
```

In the last `autocmd` statement above, dictionaries and thesauruses are
configured for the `text` file type, but spell-check is disabled by
default.

_lexical_ enables spell-check by default for buffers in which it is
initialized. You can change that default setting in your `.vimrc`:

```vim
let g:lexical#spell = 1         " 0=disabled, 1=enabled
```

### Spell-check language configuration

Vim’s global `spelllang` (note three `l`s) may already specify a default
language. You can query it with a simple command:

```vim
:echo &spelllang
=> ‘en’
```

If desired, you can be more specific, overriding the global `spelllang` in
your `.vimrc`:

```vim
let g:lexical#spelllang = ['en_us','en_ca',]
```

### Thesaurus configuration

If you don’t have one already, download a thesaurus, such as [Moby
Thesaurus][1] from Project Gutenberg and extract the `mthesaur.txt` file.
By default _lexical_ will look for it at the following path:

```vim
let g:lexical#thesaurus = ['~/.vim/thesaurus/mthesaur.txt',]
```

* You can specify multiple paths to thesauruses in the list.

[1]: http://www.gutenberg.org/ebooks/3202 "Moby Thesaurus List by Grady Ward"

### Dictionary configuration

On Unix-based systems (including OS X) the dictionary will default to:

```vim
let g:lexical#dictionary = ['/usr/share/dict/words',]
```

* You can specify multiple paths to dictionaries in the list.

## Commands

Vim offers many standard key mappings for spell-checking and completion.

### Spell-check commands

These are the _Normal_ mode commands:

* `]s`			- Move to next misspelled word after the cursor.
* `[s`			- Like `]s` but search backwards
* `]S`			- Like `]s` but only stop at bad words, not at rare words or words
  for another region.
* `[S`			- Like `]S` but search backwards.

With the following key mappings you can use Visual mode selection to select the
characters (including whitespace). Otherwise the word under the cursor is used.

* `zg`			- Mark as a good word
* `zw`			- Like `zg` but mark the word as a wrong (bad) word.
* `zug`     - Unmark as good word
* `zuw`     - Unmark as wrong (bad) word 

* `z=`			- For the word under/after the cursor suggest correctly spelled words
* `1z=`			- Use the first suggestion, without prompting
* `.`       - Redo - repeat last word replacement

* `:spellr` - Repeat the replacement done by `z=` for all matches with the
  replaced word in the current window

For spelling suggestions while in _Insert_ mode:

* `«CTRL-X» «CTRL-S»` - suggest spelling, using `«CTRL-P»` and `«CTRL-N»` to navigate.

For a convenient pop-up list of suggestions from _Normal_ mode, you can map an 
available key of your choice in your `.vimrc`:

```vim
let g:lexical#spell_key = '<leader>s'
```

This buffer-scoped mapping is strictly opt-in. No key is mapped by default.

### Thesaurus commands

For thesaurus lookup while in _Insert_ mode:

* `«CTRL-X» «CTRL-T»` - thesaurus lookup, using `«CTRL-P»` and `«CTRL-N»` to navigate.

For convenient _Normal_ mode thesaurus lookup from the cursor position,
you can map an available key of your choice in your `.vimrc`:

```vim
let g:lexical#thesaurus_key = '<leader>t'
```

This buffer-scoped mapping is strictly opt-in. No key is mapped by default.

### Dictionary commands

For dictionary completion while in _Insert_ mode:

* `«CTRL-X» «CTRL-K»` - dictionary completion, using `«CTRL-P»` and `«CTRL-N»` to navigate.

For convenient _Normal_ mode dictionary lookup from the cursor position,
you can map an available key of your choice in your `.vimrc`:

```vim
let g:lexical#dictionary_key = '<leader>k'
```

This buffer-scoped mapping is strictly opt-in. No key is mapped by default.

### Define your own commands

Sometimes you need a highly-customized environment for spell-check and
completion. You can define your own commands in your `.vimrc` to meet that
need. For example:

```vim
command -nargs=0 LexMed call lexical#init({ 
                    \ 'spell': 1, 
                    \ 'spelllang':  ['en', 'medical'],
                    \ 'dictionary': ['~/.vim/dictionary/medical_terms.txt',
                    \                '/usr/share/dict/words',
                    \               ],
                    \ 'thesaurus':  ['~/.vim/dictionary/medical_synonyms.txt',
                    \                '~/.vim/thesaurus/mthesaur.txt',
                    \               ],
                    \ })
```

Then to quickly configure Vim for the current buffer, enter the command:

```vim
:LexMed
```

Where you are providing an explicit value, it will use that. Where you do
not, it will fall back to your specified defaults or global settings.

## See also

If you like this plugin, you might like these others from the same author:

* [vim-quotable](http://github.com/reedes/vim-quotable) - extends Vim to support typographic (‘curly’) quotes
* [vim-pencil](http://github.com/reedes/vim-pencil) - Rethinking Vim as a tool for writers
* [vim-thematic](http://github.com/reedes/vim-thematic) — Conveniently manage Vim’s appearance to suit your task and environment 
* [vim-litecorrect](http://github.com/reedes/vim-litecorrect) - Lightweight auto-correction for Vim
* [vim-colors-pencil](http://github.com/reedes/vim-colors-pencil) — A color scheme for Vim inspired by IA Writer

## Future development

If you’ve spotted a problem or have an idea on improving this plugin,
please post it to the github project issue page.

<!-- vim: set tw=74 :-->
