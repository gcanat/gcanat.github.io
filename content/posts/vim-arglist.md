+++
date = '2026-05-11T23:00:45+02:00'
draft = true
title = 'Easy Navigation with Vim Arglist'
+++

## Introduction
In the days of fancy plugins super (or over?) charging vim to make it look
like a fully fledged IDE, I sometimes like to go back to the basics and
(re)discover how vim often already has something builtin to do things,
albeit maybe in a slightly different way.

Before LSP was a thing, [tags](https://vimhelp.org/tagsrch.txt.html#tags) were
already there to help us navigate a codebase and we also have the `:compiler`
command to run linters and populate the quickfix list with errors.
Before fuzzy finders were all the rage, we had `:edit`, `:find` and the
argument list (`:h arglist`).

## Exploring the file tree

When I explore a new codebase, I like to start by browsing the file structure
with `netrw` to get a sense of the organisation. Once I'm comfortable with the
file tree structure I barely use `netrw`.

## Edit and find
One straightforward way to open a file is with the `:edit` command. Just type
`:e ` and start typing the filename, maybe prefixed by some wildcards, then
you can hit <Tab> to see matching candidates and cycle through them. It can also
be quite handy to hit <c-d> to list names matching the current pattern. I use it
a lot to get a preview when I am unsure about some part of the file path.

Another possibility is to use the `:find` command, which allows you to search
files in `'path'`. One simple/naive way to set it up can be to `:set path=**`,
but beware that it might be very slow on gigantic file trees, for example if
your current directory is `$HOME`. One can easily come up with smarter ways of
adding only relevant directories to `'path'`, specially inside a git repository.
Indeed
or even making use of `:h findunc`.

## The power of the arglist
But I would like to suggest another option, which I find interesting: using the
`arglist`. Using the `:args` command, you can populate the arglist with a list
of files, e.g. `:ar src/gui_*.[ch]`, and navigate it with `:next` or `:prev`.
Adding one simple command, such as:
```vim
command! -nargs=1 -complete=arglist Argedit edit <args>`
```
you can now easily edit any file in the arglist. You can even add a keybinding
for quicker access if you want: `nnoremap <your-key> :Argedit<space>`
Given this, what I like to do is use `:arglocal` to define different arglist for
different windows (and therefore different tabs as well). Files are grouped
together where it makes sense: documentation only, CI/CD, some particular
module in the codebase, etc.

## Final thoughts
Once I am satisfied with the organisation of the files and windows I can save
the session with `:mksession`, so I dont have to redo it next time I work on
this code.
