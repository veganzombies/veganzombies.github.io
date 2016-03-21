---
layout:	post
title:	"BCtf - catvideo"
date:	2016-03-21 00:00:00
categories: "writeups"
tags:
- bctf
- stego
- forensics
excerpt: "Kitty! :D 😺😺😺"
author: "anonymous_zombie"
---

> [cat_video.mp4](https://drive.google.com/file/d/0B6h1j8zoGOHyQk9vNUg4dEltQk0/view?pref=2&pli=1)

- Points: 150

After downloading a copy of the video file, we are disappointed to find that there are no cats(😖)!  But an especially diligent zombie caught something that looks like two lines of text in the first second or so of scrobbling the video.  With the brackground noise changing each frame, we realized that we could diff the frames to get some idea of what the static content is.

![Diff]({{ site.url }}/writeups/bctf2016/catvideo/diff.jpg)

Here, you can clearly see the two lines of text.  Searching on the lower line turns up a video that is the cover file.

[![Maru](http://img.youtube.com/vi/N-CSBFdDZsw/0.jpg)](http://www.youtube.com/watch?v=N-CSBFdDZsw "When Maru has a toy in his mouth, he makes bread.")

Kitty! :D 😺😺😺 At this point, we grabbed a raw 360p copy of this video.  Then using ffmpeg we extracted frames from each video.

`ffmpeg -i cat_video.mp4 -r 1/1 stego/$filename%03d.jpg`

The first pass attempt was to try to go frame by frame between the two videos, diffing (XOR) to cancel out the cover file (Maru) presumably leaving us with just the data containing the flag: this does in fact remove the cover file, unfortunately what is left is static.  The next pass was to iterate over the provided video's frames in pairs ((1,2),(2,3),...(n-1,n)) diffing the pairs.

```
for i in `seq 2 9`; do convert stego/00$(expr $i - 1).jpg stego/00$i.jpg -fx "(((255u)&(255(1-v)))|((255(1-u))&(255v)))/255" noncover/00$i.jpg; done
```

007.jpg (stego/006.jpg ^ stego/007.jpg) has text in the flag format in the bottom of the frame.

![Flag!]({{ site.url }}/writeups/bctf2016/catvideo/flag.jpg)

Flag: BCTF{cute&fat_cats_does_not_like_drinking}
