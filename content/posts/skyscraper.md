+++
date = '2026-05-31T14:28:09+03:00'
draft = false
title = 'Skyscraper'
+++

[Skyscraper](https://github.com/degD/skyscraper) is a small Flask application that extracts 
selected regions from video frames and compiles them into a PDF. It is designed for videos 
where a small control area changes when the main content changes. 

For example, a video may consist of 10 different images, with a counter at the top right
corner. This application can be used to extract and compile a PDF from those 10 images
by checking the counter.

The application lets you select:

1. A start and end time.
2. The region to extract from each frame.
3. A control region used to identify new content.

## Notes

- `process.py` uses a similarity threshold of `0.97` to determine whether a control frame is new.
- `buildpdf.py` places up to six images on each A4 page.
- Rectangle coordinates are stored in video-pixel coordinates rather than canvas coordinates.
- The workflow is intentionally manual because fully automatic extraction is unreliable across different videos.
- Parameter tuning may be required for each video.

## Acknowledgments

This project began as an experiment while I was practicing music from YouTube 
tutorials that displayed sheet music alongside the lesson. The tool was created 
for educational purposes. It is not intended for redistributing copyrighted material.
