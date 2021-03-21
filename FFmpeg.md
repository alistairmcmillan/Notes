# FFmpeg Notes

## How to make an MP4 from a series of PNGs

    ffmpeg -r 30 -pattern_type glob -i '2021-03-13T*.png' -c:v libx264 -vf fps=30 -pix_fmt yuv420p output.mp4

    -r to set input framerate
    -pattern_type to set input name matching (set to glob because filenames aren't sequential)
    -c:v to set video codec
    -vf to set output framerate (not really necessary unless different from input framerate)
    -pix_fmt to set pixelformat (set to yuv420p so "ensure compatibility so crappy players can decode the video" https://trac.ffmpeg.org/wiki/Slideshow
    
