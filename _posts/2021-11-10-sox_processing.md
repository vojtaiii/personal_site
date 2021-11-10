---
title: "Useful batch audio files processing using SOX"
date: 2021-10-12T21:00:00-04:00
categories:
  - tools
tags:
  - wav
  - resample
  - sox
  - audio processing
---

Here I store some pretty useful commands when performing audio analysis and refinement on a larger sample of files. The one and only [SOX](soxurl) is used as a tool, of course.

### Resample multiple files

```shell
for /r %i in (*.wav) do sox "%~ni.wav" "16khz\%~ni.wav" rate 16k
```


[soxurl]: http://sox.sourceforge.net/
