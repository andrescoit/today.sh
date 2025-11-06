# today.sh

A minimal note-taking script with relative paths and dates, and with regex search.

## Usage

```
$ today -h
usage: today [relative path] [day offset(s) | ISO date(s)]*
usage: today [relative path] search [ripgrep options] [regex]

env: $WORKFOL # relative path root
requires: ripgrep, date, vim, bash

currently WORKFOL=/home/user/Documents/Work
```

## Basic Usage

`today`

This will open vim at `./notes/20XX-XX-XX.md`, today's date given by `date +%Y-%m-%d` or `date --iso`.

## Paths
If you don't specify a path it'll simply write to `./notes` directory in the current folder.

When you specify a path, it'll look relative to the `$WORKFOL` environment variable first, otherwise it'll try relative to the current directory.

## Dates
`today` is equivalent to `today 0`.

You can specify a date offset, such as `today -1` for yesterday.

Also, today can understand absolute dates, such as with `today YYYY-MM-DD`.

Vim lets you switch between multiple open documents with `:prev` and `:next`.

If you really want to, you can also use some arithmetic expressions such as `today 30*3+2`, but it won't be any better than bash arithmetic expansion because that's what I'm using.

## Search
`today [path] search -i "match"`

See ripgrep's documentation for all the options.

You can change the number of lines it prints around each match by changing the `SEARCH_CTX` environment variable, otherwise it'll use 10.

## Example

Lets say you have a directory structure:
```
.
..
example_dir
  - example_subdir
```
You set the variable `WORKFOL=/home/user/example_dir`.

The paths in your arguments will be relative to `/home/user/example_dir`.

If you call `today example_subdir 20XX-XX-XX` it will:
1. create a folder at `/home/user/example_dir/example_subdir/notes`.
2. open vim at `/home/user/example_dir/example_subdir/notes/20XX-XX-XX.md`.
```
.
..
example_dir
  - example_subdir
    - notes
      20XX-XX-XX.md
```
Then lets say you write "ABC", save and quit.

In order to find a match in your notes you can call `today example_subdir search A`,
or `today example_subdir search -i a` for a case-insensitive search.

## Installation

Copy `today` to a location in your `$PATH`.

I use `~/.local/bin/`, which I added to `$PATH` in my `~/.bashrc` file.

If you're lazy or have a simple setup, you can just copy/paste this into your bash terminal:
```bash
git clone https://github.com/andrescoit/today.sh && \
    mkdir -p ~/.local/bin && \
    cp today.sh/today ~/.local/bin && \
    echo "export PATH=\"$PATH:$HOME/.local/bin\"" >> ~/.bashrc && \
    . ~/.bashrc && \
    rm -rf today.sh && \
    echo Done!
```
