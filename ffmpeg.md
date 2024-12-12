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

## How to trim a video

    ffmpeg -ss 00:00:10 -to 00:00:50 -i input.mp4 -c copy output.mp4

    - ss         the start of the desired clip in HH:MM:SS format
    - to         the end of the desrired clip in HH:MM:SS format
    - c copy     tell ffmpeg not to reencode the audio & video, just copy them straight across

## How to trim a video while maintaining subtitle track

    ffmpeg -ss 00:00:30 -to 00:00:40 -i input.mp4 -c:v copy -c:s mov_text output.mp4
    
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
    for i in 2024*.ts; do ffmpeg -i $i -c:v libx264 -pix_fmt yuv420p ${i%.*}.mp4; done

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

    Note: this process speeds up the output video by dropping frames

## How to speed up video and audio

Speed up video and audio by four times. atempo has a max paramter of 2; hence the repeated commands.

    ffmpeg -i input.mp4 -filter:v "setpts=0.25*PTS" -filter:a "atempo=2.0,atempo=2.0" -r 30 output.mp4

## How to add overlay text to a video

    ffmpeg -i input.mp4 -vf "drawtext=fontfile=/System/Library/Fonts/Helvetica.ttc:text='Hello World':fontcolor=white:fontsize=24:box=1:boxcolor=black@0.5:boxborderw=5:x=(w-text_w)/2:y=(h-text_h)" -codec:a copy output.mp4

    ffmpeg -r 5 -pattern_type glob -export_path_metadata 1 -i '2023-06-03*.jpg' -c:v libx264 -pix_fmt yuv420p -vf "drawtext=text='%{metadata\:lavf.image2dec.source_basename\:fuck}':fontcolor=white:fontsize=24:box=1:boxcolor=black@0.5:boxborderw=5:x=(w-text_w-60)/2:y=h-text_h" output.mp4

    ffmpeg -r 10 -pattern_type glob -export_path_metadata 1 -i '2024*.jpg' -c:v libx264 -pix_fmt yuv420p -vf "drawtext=text='%metadata\:lavf.image2dec.source_basename\:fuck}':fontcolor=white:fontsize=50:box=1:boxcolor=black@0.5:boxborderw=5:x=(w-text_w+20)/2:y=10" short.mp4

## Combine nine videos into a single grid video

    ffmpeg \
        -i one.mp4 \
        -i two.mp4 \
        -i three.mp4 \
        -i four.mp4 \
        -i five.mp4 \
        -i six.mp4 \
        -i seven.mp4 \
        -i eight.mp4 \
        -i nine.mp4 \
        -filter_complex \
            "[0:v]  [1:v]  [2:v]  hstack=inputs=3 [row1]; \
             [3:v]  [4:v]  [5:v]  hstack=inputs=3 [row2]; \
             [6:v]  [7:v]  [8:v]  hstack=inputs=3 [row3]; \
             [row1] [row2] [row3] vstack=inputs=3 [out]" \
        -map "[out]" grid.mp4

## Add border around video

    ffmpeg -i input.mp4 -filter_complex "[0]pad=w=100+iw:h=50+ih:x=50:y=25:color=black;" output.mp4

    - 'w=100+iw' take the initial width and add 100 pixels
    - 'h=50+iw' take the initial height and add 50 pixels
    - 'x=50:y=25' place the original video at 50, 25 coordinates
    - 'color=black' the color of the border

## Add x seconds of white frames to the end of a video

    ffmpeg -f lavfi -i color=c=white:s=1280x720:r=30:d=60 white.mp4

    - to create a 60 seconds video of 1280 x 720 white running at 30 frames per second

    Then use the concat method above to join the white video to the beginning or end of the original video

## Pan across existing video clip

    ffmpeg -i input.mp4 -vf "crop=in_w*0.56:in_h*1.0:(in_w*0.025)*t:0" output.mp4

    - 'in_w*0.56' sets the start width to 56% of the original width
    - 'in_h*1.0' sets the start height to 100% of the original height
    - '(in_w*0.025)*t' sets the start horizontal position to (2.5% of the original width) multiplied by the timestamp
    - '0' keeps the start vertical position to 0

    The aim of this being to create a square video panning left to right across a 20 second 3840*2160 video

## Changing frame rate of a video

    ffmpeg -i input.mp4 -filter:v fps=30 output.mp4

