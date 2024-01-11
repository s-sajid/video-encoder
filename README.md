# Video Encoder

A video encoder that achieves 90% compression by converting frames and implementing delta encoding, run-lenth encoding, and DEFLATE compression.

## Usage

To start the encoding application, run the following command:

`cat video.rgb24 | go run main.go`

The decoded video can then be played with ffplay using the following command:

`ffplay -f rawvideo -pixel_format rgb24 -video_size 384x216 -framerate 25 decoded.rgb24`

## Description

This program performs the following steps:

- Reads raw video frames from stdin in a rgb24 format.

- Converts each frame to a YUV420 format, separating luminance (Y) and chrominance (UV) based on the ITU-R standard.

- Downscales the U and V components by averaging adjacent pixels, reducing storage.

- Combines Y, U, and V values into a byte slice in a planar format, achieving better compressibility.

- Computes the delta between consecutive frames, storing keyframes and delta frames (predicted frames).

- Compresses delta frames using run-length encoding to further reduce data size.

- Applies DEFLATE compression to achieve final compression

- Decodes the compressed data, recreating video frames.

- Converts YUV frames back to RGB format.
