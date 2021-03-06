---
layout: post
title: Smart Buffering For Progressive Video
disqus_url: http://timrobles.com/blog/entry/smart_buffering_for_progressive_video/
---

The ideal video experience
--------------------------

The easiest way to serve flash video is to progressively download a FLV. Deploy the FLV just like any other file without the need of a media server, but you don't get the added benefit of seek ahead and video quality adjustment based on download speed. It's the developer's responsibility to ensure that the best possible video experience is created even though a fancy video server isn't available-- usually due to timing and cost. By default, the bufferTime of the NetStream class is 0.1 seconds, prompting us to choose a sufficient buffer time. Here, I present a general strategy for calculating an appropriate buffer time to optimize the progressive download for the user. An ideal video experience might have the following qualities:

* No stoppage in playback (the buffer never becomes empty)
* Minimize the amount of time the user has to wait until the video starts playing

Calculating the correct buffer time
-----------------------------------

We also know the size of the FLV we want to download, and can extrapolate the average download speed for our site. Most microsites will have a lot of external files loaded at runtime, all of which usually piped through a central loader. Say for example we are downloading 1.5 MB (several files) and it takes 15 seconds to download it, resulting in a 102.4 KBps download speed. We can then estimate that for a 10 MB video file, it will take 100 seconds to download it. Furthermore, suppose our 10 MB video is about 90 seconds long. From there we can extrapolate that if the user buffers 10 seconds, then based on their average download speed, the buffer should never be empty. We want the estimated download time to equal the duration of the video.

You can extrapolate the buffering time even further by having different bitrates of FLVs on the server and selecting which file to download dynamically. You can encode varying quality of videos that you can serve up based on the download speed, like video_low.flv (10 MB, 750 kbps bitrate) , video_medium.flv (1.5 mbps bitrate), etc. You can pick a standard buffering time for your target users, for example, the target buffering time could be 15 seconds. You can then choose the highest quality video that you can serve without buffering for longer than 15 seconds, based on their average download speed. This way you can mimic the quality degradation of Flash media servers.

Additionally, there are some edge cases to consider. You might want to set a maximum buffering time to be half the video duration, or even a fixed amount, like 30 seconds. This is to accommodate really slow download speeds, because at that point, it might just be better for the user to at least see some of the video and have buffer empty rather than them have a super long wait without seeing anything. Even though having the buffer empty is not ideal, I think most users are used to manually pausing the video and waiting for the video to buffer. There's also the case of the time to download being less than the duration of the video. If that's the case, then we can practically start playing the video immediately, but we'll also need a minimum buffering time, say 5 seconds.

It's just an estimate
---------------------

One last thing to keep in mind is that the average download speed calculation is just that--an average as observed by the Flash application. It doesn't necessarily take in to account spikes or lags in the user's connection. Some of the proprietary video players on some network television channel websites require an extra plugin in addition to the Flash, which might tap into the overall network/download capacity rather than the one just observed by the Flash application itself. If we want to enhance our calculation we could add an amount of variance or error correction to our estimated download speed.

Progressive video doesn't have to suck
--------------------------------------

In general, I think the point is that we shouldn't be handcuffed when using progressive video. These are just some methodologies that you can use to enhance the experience for the user. Oftentimes, I think we get caught up in the design of playback controls, or fullscreen capabilities as part of the refinement of a video player. But in reality, the download and buffering time has the biggest effect on the end user wanting to watch the video or not, and shouldn't be overlooked.