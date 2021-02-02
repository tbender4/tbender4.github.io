---

title: "Video Capture with Datapath Cards and RGBEasy OBS Plugin"
categories: [retro, video capture]
excerpt_separator: "<!--more-->"
# last_modified_at: 2021-02-02
tags: [retro, gaming, datapath, video capture]
classes: wide


---

*Quick primer of Datapath VisionRGB Capture Cards here*

When capturing and streaming older video game consoles with Datapath Capture Cards in OBS Studio, [beeanyew'S RGBEasy plugin](https://github.com/beeanyew/datapath-RGBeasy) is an improvement over the default OBS DirectShow implementation as it's easier to configure and has oversampling support (explained later).

## Quick Comparison

Below is the original Playstation with both stock analog RGB video via SCART (left) and lossless digital video with the [PS1Digital](https://www.black-dog.tech/ps1digital.html) mod. (right).

![image](/assets/images/2021-01-29-video-capture-with-datapath-and-rgbeasy-obs-plugin/ps1_gt.png)

The RGB source is configured with 4x oversampling using the RGBEasy Plugin and integer/point upscaled to 1280x960 resolution. The PS1Digital upscaling is integer scaled to the same resolution and represents a "best-case scenario" of video quality.

Video capture of older consoles through the Datapath E1 is very impressive! At a glance the final result is very close to emulation picture quality.

## Normal Sampling Configuration

For game consoles that have a 240p or otherwise non-standard resolution, Datapath cards are not a plug-and-play solution for video capture. It requires game-specific configuration in the RGBEasy plugin to look correct.

*TODO: Do updated RGBEasy+OBS capture writeup.*

Configuration tutorial is available on the [r3 wiki](https://r3.fyi/Datapath/Capture240p).

## Oversampling

Oversampling is an alternative configuration by multiplying the Horizontal Resolution and H. Size by the same constant. It has a few advantages over doing a normal sample and integer-scale in software.

As an example, here's a comparison of capture settings for Gran Turismo:

|  | Normal | 4x Oversample |
| --- | --- |
| H. Resolution | 256 | 1024 *(4\*256)* |
| V. Resolution | 240 | 240 |
| H. Size | 341.**25** | 1365 *(4\*341.25)* |

The final resolution must also be scaled back to a proper 4:3 ratio image. This can be achieved using the Scaing/Aspect Ratio filter in OBS.

### Why to Oversample

- Serves as a workaround for if a game's native resolution requires a fractional H.Size (ex: 427.5 can be multiplied by 2 to be 855 instead).
- Can be a workaround for games that switches between two resolutions by using an H. Resolution that has both in-game resolutions. This is described in detail on the Genesis page of the [r3 wiki](http://r3.fyi/VGC/GEN)
- **Significantly** increases the tolerance for tuning Phase to get the picture right. 


### A Disadvantage
- Has a slight amount of blur when scrutinized at full resolution.
- This feature is limited to the RGBEasy API and does **not** work in DirectShow (labeled as Video Capture Devices in OBS), making the RGBEasy plugin a requirement.

It's worth attempting both configurations to see if the game look best at its sharpest or with the conveniences afforded by oversampling.


{% comment %}
![image](/assets/images/2021-01-29-video-capture-with-datapath-and-rgbeasy-obs-plugin/gen_sor2_fbx.png)

Here's a comparsion between normal settings and oversampling on a Sega Genesis.
Screenshot credit to FirebrandX.

As an H.265 video, the differences between the two are less pronounced as it'll be inevitably compressed.

And as a bonus, here's the same PS1 as earlier when hooked up to an OSSC.
{% endcomment %}

