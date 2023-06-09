# FFmpeg Notes

## How to make an MP4 from a series of PNGs

    ffmpeg -r 30 -pattern_type glob -i '2021-03-13T*.png' -c:v libx264 -pix_fmt yuv420p output.mp4

    -r               to set input framerate
    -pattern_type    to set input name matching (set to glob because filenames aren't sequential)
    -c:v             to set video codec
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

## How to operate on a bunch of files

    for i in *.avi; do ffmpeg -i "$i" "${i%.*}.mp4"; done
    for i in input-0*.mp4; do ffmpeg -i "$i" -filter:v 'setpts=0.03*PTS' "${i%.*}-faster.mp4"; done
    for i in input-0*.mp4; do ffmpeg -i "$i" -vf "drawtext=fontfile=/System/Library/Fonts/Helvetica.ttc:text='${i%.*}':fontcolor=white:fontsize=35:box=1:boxcolor=black@0.5:boxborderw=5:x=(w-text_w)/2:y=(h-text_h)" -codec:a copy "${i%.*}-text.mp4"; done

## How to extract audio track from MP4 file

    ffmpeg -i source.mp4 -vn -acodec copy output.aac

## How to import audo track into MP4 file

    ffmpeg -i input.mp4 -i output.aac -map 0 -map 1:a -c:v copy -shortest output.mp4

    - map 0      takes everything from input 0, the input.mp4 file
    - map 1:a    takes only the audio track from the output.aac file
    - c:v copy   tells ffmpeg to just copy the video input, don't bother reencoding it
    - shortest   tells ffmpeg to make the output.mp4 file the same length as the shortest input

## How to crop video to a specfic resolution

    ffmpeg -i input.mp4 -filter:v "crop=width:height:x:y" output.mp4

    - width     target width of output video
    - height    target height of ouptut video
    - x         x coordinate to start crop within input video
    - y         y coordinate to start crop within input video

## How to speed up video

    ffmpeg -i input.mp4 -filter:v "setpts=0.5*PTS" output.mp4

    - setpts    an expression to change presentation timestemp (PTS) of each frame

    Note: this process speeds up the output video by dropping frame

## How to add overlay text to a video

    ffmpeg -i input.mp4 -vf "drawtext=fontfile=/System/Library/Fonts/Helvetica.ttc:text='Hello World':fontcolor=white:fontsize=24:box=1:boxcolor=black@0.5:boxborderw=5:x=(w-text_w)/2:y=(h-text_h)" -codec:a copy output.mp4

    ffmpeg -r 5 -pattern_type glob -export_path_metadata 1 -i '2023-06-03*.jpg' -c:v libx264 -pix_fmt yuv420p -vf "drawtext=text='%{metadata\:lavf.image2dec.source_basename\:fuck}':fontcolor=white:fontsize=24:box=1:boxcolor=black@0.5:boxborderw=5:x=(w-text_w-60)/2:y=h-text_h" output.mp4

