# how sean uses comfyui

This is a work in progress and is just some pesonal notes for now.

## Tooling

* [ComfyUI](https://github.com/comfyanonymous/ComfyUI)
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
* [Real-ESRGAN](https://github.com/xinntao/Real-ESRGAN)
    * I rename it to a shorter executable (`realesrgan.exe`) then add the folder to my windows PATH so that I can use it anywhere from the command prompt. Make sure output directory is created before hand.
    * Don't scale to 4x if you plan on interpolating with GMFSS because it won't complete, they'll be too big (at least for my 12GB 3080 any ways).
    * Example: `realesrgan -i input_sequence\ -o output_scaled\ -n realesrgan-x4plus -s 2`
* [Adobe Premiere](https://www.adobe.com/ca/products/premiere.html). I used Sony Vegas for a little while but it's garbage compared to premiere. Take advantage of student/educator pricing if you can, you save a lot of money!
* [python 3.10](https://www.python.org/downloads/) (some tooling doesn't yet work with 3.11 and later)
* [GMFSS_Fortuna](https://github.com/98mxr/GMFSS_Fortuna)
    * create a python venv: `python -m venv venv`
    * `pip install -r requirements.txt`
    * I may have done some other things to get nvidia cuda working, I don't remember. I may have installed torch-gpu using pip?

## Post process

I post process on the CLI with real-esrgan and GMFSS Fortuna instead of using nodes in ComfyUI because ComfyUI takes absolutely forever to save PNGs for each of these steps. It also makes the workflow less risky in case there's a failure after it's sampled the frames.

1. Render frames out with Adobe Premiere. Make sure that the transparency isn't included and it's RGB (not RGBA) or else there will be problems when interpolating the frames.
2. Upscale frames with [Real-Esrgan](https://github.com/xinntao/Real-ESRGAN) to 2x (4x is too big for GMFSS Fortuna).
    1. `realesrgan -i input\ -o output\ -n realesrgan-x4plus -s 2`
    2. Sip a coffee while listening to the coils in my PC make strange buzzing sounds.
3. Interpolate frames to 60fps with GMFSS Fortuna
    1. Move scaled frames directory to git folder of GMFSS Fortuna
    2. All files must be named "####".png. If there are any letters in the filenames then it will fail.
    3. `venv\Scripts\activate`
    4. `python inference_video.py --img=input\ --scale=1.0 --multi=2 --union`
    5. Sip a coffee while listening to the sound of my GPU fans.