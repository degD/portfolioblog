+++
date = '2026-06-07T00:22:45+03:00'
draft = false
title = 'GRAMCHECK'
+++

I believe active practice is one of the most effective ways of learning. However, practice
is hard without regular feedback. [gramcheck](https://github.com/degD/gramcheck) 
is a text-analysis tool that examines a text, sentence by sentence, and provides feedback 
on its syntax and semantics by using LLMs. It is also 
[available on PyPI](https://pypi.org/project/gramcheck/).

LLMs are probabilistic and while they can produce inaccurate feedback, they are accessible
and they are capable of identifying many common language errors. A typical language practicing 
workflow is submitting a text with a short prompt, correcting the reported issues, and 
submitting the revised text again. This process was manual and slow. `gramcheck` is built
to automate this process.

This project uses direct API access through Google, which offers a 
[generous free tier](https://ai.google.dev/gemini-api/docs/pricing#gemini-3.1-flash-lite).
However, local LLM support is planned for the future. The tool buffers the complete response 
before displaying it, so there may be a delay after submitting text. It also uses a predefined 
seed, which makes responses deterministic for identical input.
