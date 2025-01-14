---
title: New Example using API! 💥
description: >-
  Use 'CIQRCodeGenerator' to create a QR code for the FFmpeg homepage, given as
  complete and escaped command-line for Apple’s standard bash shell.

  More info on the FFmpeg documentation:
  https://ffmpeg.org/ffmpeg-filters.html#coreimagesrc-1
date: '2021-05-20T06:01:44Z'
categories:
  - Video Filters
tags:
  - lavfi
  - video-source
  - coreimage
author: Elad Gariany <elad@gariany.com>
thumbnail_url: https://ffmpeg-by-example.s3.amazonaws.com/QRCode.png
example_code: |-
  ffmpeg -y -f rawvideo -pix_fmt rgb32 -s 1920x1080 \
    -i /dev/zero -r 30 -filter_complex \
      "
        movie='square.png'[sqr];
        [0:][sqr]overlay=(main_w-overlay_w)/2:(main_h-overlay_h)/2[canvas];
        [canvas]chromakey=0x008000:blend=0:similarity=0.15[canvas2];
        [0:][canvas2]overlay[canvas3];
        movie='input.mp4',scale=1920:1080[i1];
        [0:][i1]overlay[i2];
        [i2][canvas3]overlay[mix1];
        [0:][mix1]overlay[o]
      " \
    -map "[o]" -c:v libx264 -crf 31 \
    -frames:v 300 masked_video.mp4
---
