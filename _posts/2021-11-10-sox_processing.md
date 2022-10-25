---
title: "Batch audio files processing"
date: 2021-10-12T21:00:00-04:00
categories:
  - tools
tags:
  - wav
  - resample
  - sox
  - audio processing
---

Here I store some pretty useful commands when performing audio analysis and refinement on a larger sample of files. The SOX tool can be downloaded [here](soxurl).

### Resample multiple files

```shell
for /r %i in (*.wav) do sox "%~ni.wav" "16khz\%~ni.wav" rate 16k
```

### Change the bit resolution of multiple files

```shell
for %i in (*.wav) do sox "%~ni.wav" -b 16 "16bit\%~ni.wav"
```

### Normalize multiple audio files

Specify only the maximum aplitude (here -1.0 dB) bellow the clipping level (0.0 dB). The results are stored in a folder `normalized` on the same level as original.

```shell
for /r %i in (*.wav) do sox "%~ni.wav" "..\normalized\%~ni_norm.wav" norm -1.0
```

### Extract audio from a video file

The best tool for that is ffmpeg, another command line interface. Ffmpeg can be downloaded [here](ffmpegurl).

```shell
ffmpeg -i video_file.mp4 audio_file.wav
```

Ffmpeg offers a much wider range of utilities for working with video/audio files.

### Rename multiple files in the folder at once

The script asks for a root word and then renames all *.wav files in the current folder to root_1, root_2, root_3...

```shell
@echo off
:: Rename all the files in the folder with the given extension to root_x where x 
:: is the file order
:: Vojtech Illner, November 2021
TITLE Rename mutliple files

:: Enable delayed extension for loop index tracking
setlocal enableextensions enabledelayedexpansion

set /p root="Enter the root word: "
ECHO renaming files...

set /a k=0
for %%i in (*.wav) do (
    ren "%%~i" "%root%_!k!%%~xi" 
    set /a k += 1
)
endlocal
ECHO completed.
```

[soxurl]: http://sox.sourceforge.net/
[ffmpegurl]: https://ffmpeg.org/
