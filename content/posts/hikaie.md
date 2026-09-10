+++
date = '2026-02-20T16:52:03+03:00'
draft = false
title = 'Hikaie'
+++

[Hikaie](https://gitlab.com/den.ege.der/download-a-story) is an unofficial, 
open-source Storytel client created as an experiment in building a web 
application without using any frameworks. It is licensed under 
the MIT License, so the code can be freely copied, modified, and shared.

The design is inspired from the 
[DAO pattern](https://en.wikipedia.org/wiki/Data_access_object). App is
an SPA (Single-Page Application) using a custom show-and-hide routing.

## Tech Stack

- **Language:** TypeScript
- **Bundler:** Vite
- **Mobile wrapper:** Capacitor.js
- **Routing:** Custom

## Credits

The project was largely inspired from 
[Storytel Desktop](https://github.com/debba/storytel-player), which I adapted 
the login and the download logic. I discovered the search endpoint through trial 
and error. I investigated the API using [mitmproxy](https://mitmproxy.org/) on an 
Android emulator as well.

## Lessons Learned

- SPAs are more common than MPAs as managing state synchronization is easier.
- While Capacitor.js is useful for packaging web sites as mobil applications, it is
harder to access native features from WebView. For example, audiobook playback
configurations, large file downloads, background playing.
- Managing component states with event-based coding is hard to maintain. Objects to
represent interface elements are introduced. This approach was inspired by the 
[DAO pattern](https://en.wikipedia.org/wiki/Data_access_object).
