---
date: '2026-09-10T18:50:00+03:00'
draft: false
title: 'StoryCLI'
---

[StoryCLI](https://github.com/degD/storycli) is an unofficial CLI tool for 
downloading audiobooks from Storytel. An active Storytel account is required.
I am not responsible for any issues that this tool could cause. It is MIT
licensed. Download from PyPI: `pipx install storycli`

**Search an Audiobook:**

    storycli -q <search-query>
    storycli -q Dune
    storycli --query Dune

    ID       LENGTH    TITLE 
    -------  --------  ---------------
    ...
    2759046   18:36:1  Paul of Dune
    2836812  17:19:18  The Winds of Dune
    2835170  19:31:19  Sandworms of Dune
    ...

**Download an Audiobook:**

    storycli -D <book-id> -o <mp3-save-path>
    storycli -D 2759046 -o ~/Downloads/2759046.mp3
    storycli --download 2759046 -o ~/Downloads/2759046.mp3

## Future Improvements

* Prevent logging in everytime by storing login tokens.
* Add book description preview.
* Better UX.
* Standalone Storytel TUI with builtin player.
* Better error message management.
