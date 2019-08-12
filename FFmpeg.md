# FFmpeg Notes

## How to make an MP4 from a series of PNGs

- `ffmpeg -r 30 -i waverley%04d.png -c:v libx264 -v fps=30 -pix_fmt yuv420p output.mp4`
