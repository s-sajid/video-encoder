# Video Encoder

A video encoder that achieves 90% compression by converting frames and implementing delta encoding, run-lenth encoding, and deflate compression.

https://github.com/s-sajid/video-encoder/assets/88870507/292f98e1-175d-47cf-aed2-820b6d022129

## Usage

First, convert the `.mp4` file into a `.rgb` using:

`ffmpeg -i koala.mp4 -c:v rawvideo -pix_fmt rgb24 koala.rgb`

Next, to start the encoding application, run the following command:

`cat koala.rgb | go run main.go`

The decoded video can then be played with ffplay using the following command:

`ffplay -f rawvideo -pixel_format rgb24 -video_size 1080x1920 -framerate 25 koala.rgb`

To convert the `.rgb` file back into `.mp4`, run the following command:

`ffmpeg -f rawvideo -pixel_format rgb24 -video_size 1080x1920 -framerate 25 -i koala.rgb -c:v libx264 -pix_fmt yuv420p output.mp4`

Note, the parameters for these commands will change depending on user requirements.

## Description

This program performs the following steps:

- Reads raw video frames from stdin in a rgb format.

- Converts each frame to a YUV420 format, separating luminance (Y) and chrominance (UV) based on the ITU-R standard.

- Downscales the U and V components by averaging adjacent pixels, reducing storage.

- Combines Y, U, and V values into a byte slice in a planar format, achieving better compressibility.

- Computes the delta between consecutive frames, storing keyframes and delta frames (predicted frames).

- Compresses delta frames using run-length encoding to further reduce data size.

- Applies DEFLATE compression to achieve final compression

- Decodes the compressed data, recreating video frames.

- Converts YUV frames back to RGB format.

