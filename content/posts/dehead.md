+++
date = '2026-05-30T01:13:55+03:00'
draft = false
title = 'DEHEAD: Anonymization by Head Detection'
+++

I was looking for an anonymization tool for a project when I found that many existing solutions 
rely on a two-step process: detect the face, then blur or mask it.

This approach can be unreliable when a subject turns sideways, is viewed from behind or above, 
or moves significantly. To handle these cases, I built **dehead**, a small command-line tool 
inspired by [deface](https://github.com/ORB-HD/deface). Instead of detecting only the face, 
it uses YOLO to detect the entire head and applies a mask to the detected region.

It uses a pretrained YOLO model from [Head-Detection-Yolov8](https://github.com/Owen718/Head-Detection-Yolov8).
If the model is unavailable from the original project, it can also be downloaded from this 
[community-hosted mirror](https://github.com/degD/dehead/releases/tag/v0.1.0). Download `best.pt` and 
place it in the project root.

## Demo Videos

- **Original:**  
  https://github.com/user-attachments/assets/cc244aa2-a66c-4439-b1ef-166a8874919e

- **Deface:**  
  https://github.com/user-attachments/assets/79d284c1-3016-482a-84ac-dfa37d1d4992

- **Dehead:**  
  https://github.com/user-attachments/assets/3b53d6c4-7ead-41d2-9f9d-88a68a25c87f

## Installation

The project uses [`uv`](https://docs.astral.sh/uv/).

1. Install `uv`.
2. Clone the repository:

   ```bash
   git clone https://github.com/degD/dehead
   cd dehead
   ```

3. Make sure Python 3.12 or newer is installed.
4. Install the dependencies:

   ```bash
   uv sync
   ```

5. Add the `bin/` directory to your `PATH`.
6. Download the model weights and place `best.pt` in the project root.
7. Run `dehead`.

## Known Issues

- Input directories are not supported, although images and videos can be passed together.
- Batch-based processing may affect performance.
- Audio is not preserved when processing videos.
- Output videos are re-encoded using `mp4v` at a fixed frame rate of 30 FPS. This may 
increase file size or affect playback speed for videos with other frame rates.
- The model weights come from a separate project and may change or become unavailable.
Model performance may also vary depending on the input. A dedicated model is planned for a future version.
- Progress reporting is not currently implemented.

## Credits

- The project is inspired by [deface](https://github.com/ORB-HD/deface).
- Model weights are sourced from [Head-Detection-Yolov8](https://github.com/Owen718/Head-Detection-Yolov8).
