======
FFmpeg
======

1. `Basics`_
2. `References & External Resources`_

`back to top <#ffmpeg>`_

Basics
======

* `Video Conversion`_, `Video Editing`_
* open-source command line tool for Linux, Windows, and OS-X
* options are written before the file they refer to
    * e.g. ``ffmpeg -framerate 5 -i IMG_%4d.jpg -shortest-codec:v mpeg4 -q:v 3 out.mp4``


Video Conversion
----------------
    * ``ffmpeg -i anyinput.xx anyoutout.xx``
    * **MP4 to MOV**
        - ``ffmpeg -i in.mp4 -acodec copy -vcodec copy -f mov out.mov``
    * **MOV to MP4**
        - ``ffmpeg -i in.mov -acodec copy -vcodec copy out.mp4``, or ``ffmpeg -i input.mov -q:v 0 output.mp4``
    * **MOV to MP4 with h265 Compression**
        - default medium preset, default 28 crf
        - ``ffmpeg -i in.mov -c:v libx265 -preset slow -crf 25 -acodec copy out.mp4``

Video Editing
-------------
    * **Cut using Start Time and Duration**
        - ``ffmpeg -i in.mp4 -ss 00:00:01 -t 00:01:00 -c copy out.mp4``
        - ``-c copy``: copy the video and audio streams without re-encoding, faster and maintain
          quality
    * **Cut using Start and End Time**
        - ``ffmpeg -i in.mp4 -ss 00:00:01 -to 00:01:00 -c copy out.mp4``

`back to top <#ffmpeg>`_

References & External Resources
===============================

* Koch, Michael. (2024). An Introduction to FFmpeg, DaVinci Resolve, Timelapse and Fulldome
  Video Production, Special Effects, Color Grading, Streaming, Audio Processing, Canon 6D,
  Canon 5D-MK4, Panasonic LUMIX GH5S, Kodak PIXPRO SP360 4K, Ricoh Theta V, Synthesizers, Image
  Processing and Astronomy Software. Available at:
  https://www.astro-electronic.de/FFmpeg_Book.pdf

`back to top <#ffmpeg>`_
