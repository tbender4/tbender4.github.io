---

title: "Video Capture with Datapath Cards and RGBEasy OBS Plugin"
categories: [retro, video capture]
excerpt_separator: "<!--more-->"
# last_modified_at: 2016-03-09T16:20:02-05:00
tags: [retro, gaming, datapath, video capture]
class: wide


---


When capturing and streaming video with Datapath Capture Cards in OBS Studio, [beeanyew'S RGBEasy plugin](https://github.com/beeanyew/datapath-RGBeasy) is an improvement over the default DirectShow implementation as it's easier to configure and has oversampling support, which is explained later.

## Quick Comparison

Below is video capture of the original Playstation with PS1Digital installed of both stock analog RGB video via SCART (left) and lossless digital video via HDMI (right).

![image](/assets/images/2021-01-29-video-capture-with-datapath-and-rgbeasy-obs-plugin/ps1_gt.png)

The RGB source is configured with 4x oversampling using the RGBEasy Plugin and upscaled to 1280x960 by integer/point scaling. The PS1Digital upscaling is integer scaled to the same resolution and represents a "best-case scenario" of video quality.

Video capture of older consoles through the Datapath E1 is very impressive! At a glance it's on-par with emulation quality.

## 1X Sampling Configuration

Detailed configuration settings and tutorial are listed on the [r3 wiki](r3.fyi).

## Oversampling

Oversampling is an alternative configuration by multiplying the Horizontal Resolution and H. Size by the same constant. It serves as a workaround for if a game's native resolution requires a fractional H.Size (ex: 427.5 can be multiplied by 2 to be 855 instead). It also drastically increases the tolerance for tuning Phase to get the picture right. This  feature is limited to the RGBEasy API and does **not** work in DirectShow sources. The one caveat of oversampling is the slightly blurrier appearance when scrutinized at full resolution. It's worth attempting both configurations to see which one looks best.

Here's a comparsion between normal settings and oversampling on a Sega Genesis.

{% comment %}
![image](/assets/images/2021-01-29-video-capture-with-datapath-and-rgbeasy-obs-plugin/gen_sor2_fbx.png)
{% uncomment %}

Screenshot credit to FirebrandX.

As an H.265 video, the differences between the two are less pronounced as it'll be inevitably compressed.

{% comment %}
And as a bonus, here's the same PS1 as earlier when hooked up to an OSSC.
{% uncomment %}

