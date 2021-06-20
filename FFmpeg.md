# FFmpeg Notes

## How to make an MP4 from a series of PNGs

    ffmpeg -r 30 -pattern_type glob -i '2021-03-13T*.png' -c:v libx264 -vf fps=30 -pix_fmt yuv420p output.mp4

    -r               to set input framerate
    -pattern_type    to set input name matching (set to glob because filenames aren't sequential)
    -c:v             to set video codec
    -vf              to set output framerate (not really necessary unless different from input framerate)
    -pix_fmt         to set pixelformat (set to yuv420p to "ensure compatibility so crappy players can decode the video")

## Export a single frame from a video

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

    fmpeg -i input.mp4 -map 0:s:0 subtitles.srt
