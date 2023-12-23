# how sean uses comfyui

This is a work in progress and is just some pesonal notes for now.

## Tooling

* [ComfyUI](https://github.com/comfyanonymous/ComfyUI)
* [ComfyUI Node Manager](https://github.com/ltdrdata/ComfyUI-Manager) to install custom nodes missing from my system.
    * It has a handy button which installs nodes in your workflow which are missing from your system.
* [ComfyUI FizzNodes](https://github.com/FizzleDorf/ComfyUI_FizzNodes) for scheduled prompts.
* [ComfyUI AnimateDiff Evolved](https://github.com/Kosinkadink/ComfyUI-AnimateDiff-Evolved) for animation
* [ComfyUI Impact Pack](https://github.com/ltdrdata/ComfyUI-Impact-Pack) for face fix.
* [sesopenko/fizz_node_batch_reschedule](https://github.com/sesopenko/fizz_node_batch_reschedule) for rescheduling my fizznode schedules for animation batches.
    * requires [go 1.21](https://go.dev/doc/install) or later.
* [ComfyUI Video Helper Suite](https://github.com/Kosinkadink/ComfyUI-VideoHelperSuite) for simpler input and output of frames.
    * Add `%H%M%S` to the folder name so that your output doesn't mash together.
* [Docker Desktop](https://www.docker.com/products/docker-desktop/) for running various tools meant for linux which aren't available on windows. Make sure you turn on intel Hyper-V or AMD's AMD-V in your bios.
* [sesopenko/youtube-dl](https://github.com/sesopenko/youtube-dl) for downloading youtube audio and video files. Needs Docker.
* [ffmpeg](https://www.ffmpeg.org/download.html)
    * Get important information such as framerate: `ffmpeg -i input_video.mp4`
    * Convert audio files to wav for use in Premiere: `ffmpeg -i input_audio.m4a output.wav`
    * Extract animation frames. Remember to include `-vf "fps=30"` (or whatever your framerate is) so that variable frame rate videos have consistent frame rates.
        `ffmpeg -i input_video.mp4 -vf "fps=30" output/%04d.png`
    * Can be used to 
* [mkvtoolsnix](https://mkvtoolnix.download/) for merging video and audio files together when required.
* [Adobe Premiere](https://www.adobe.com/ca/products/premiere.html). I used Sony Vegas for a little while but it's garbage compared to premiere. Take advantage of student/educator pricing if you can, you save a lot of money!
* [python 3.10](https://www.python.org/downloads/) (some tooling doesn't yet work with 3.11 and later)
* [ImageMagick](https://imagemagick.org/script/download.php#windows) for image resizing with good filters.
* [Smart Rename](https://github.com/chrdavis/SmartRename) for renaming large batches of files using regex and regex groups.
    * If you're new to regex, just learn about `^`, `$`, `.`, `*`, `+`, `\d` and `()`.
    * ie: `sequence(\d+).png` as the search string and `$1.png` as the output will turn `sequence0001.png` to `0001.png`.
    * handy for the picky input of GMFSS Fortuna

## Post process

I post process using [upscale_interpolate.json](comfyui_workflows/post_processing/upscale_interpolate.json).

Get [4x_foolhardy_Remacri](https://huggingface.co/FacehugmanIII/4x_foolhardy_Remacri/tree/main) for it to work.

## Copyright

Copyright (c) Sean Esopenko 2023