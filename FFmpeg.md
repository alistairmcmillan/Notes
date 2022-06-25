# FFmpeg Notes

## How to make an MP4 from a series of PNGs

    ffmpeg -r 30 -pattern_type glob -i '2021-03-13T*.png' -c:v libx264 -vf fps=30 -pix_fmt yuv420p output.mp4

    -r               to set input framerate
    -pattern_type    to set input name matching (set to glob because filenames aren't sequential)
    -c:v             to set video codec
    -vf              to set output framerate (not really necessary unless different from input framerate)
    -pix_fmt         to set pixelformat (set to yuv420p to "ensure compatibility so crappy players can decode the video")

## How to export a single frame from a video

    ffmpeg -i video.mp4 -ss 01:23:45 -vframes 1 -q:v 2 output.png
    
    - ss         the start of the desired clip in HH:MM:SS format
    - vframes    the number of frames to output
    - q:v 2      the quality of the output
    
## How to trim a video while maintaining subtitle track

    ffmpeg -i input.mp4 -ss 00:00:30 -to 00:00:40 -c:v copy -c:s mov_text output.mp4
    
    - ss              the start of the desired clip in HH:MM:SS format
    - to              the end of the desrired clip in HH:MM:SS format
    - c:v copy        tell ffmpeg not to reencode the video, just copy it straight across
    - c:s mov_text    tell ffmpeg to copy the subtitle track and encode it in "mov_text" format which works in QuickTime and VLC

## How to extract subtitle track to file

    ffmpeg -i input.mp4 -map 0:s:0 subtitles.srt

## How to concatentate multiple videos into one

Create a file called `list.txt` that lists all the files you want to concatenate with contents looking like this.

    file 'animation4 one paddle/output.mp4'
    file 'animation5 two paddles/output.mp4'
    file 'animation6 four paddles/output.mp4'
    file 'animation8 eight paddles/output.mp4'

Then run the following command to concatenate them.

    ffmpeg -f concat -safe 0 -i list.txt -c copy output.mp4

## How to extend the last frame of the video for X seconds

    ffmpeg -i input.mp4 -vf tpad=stop_mode=clone:stop_duration=2 output.mp4

## How to reverse a video

    ffmpeg -i input.mp4 -vf reverse output.mp4

## How to convert a bunch of files

    for i in *.avi; do ffmpeg -i "$i" "${i%.*}.mp4"; done
