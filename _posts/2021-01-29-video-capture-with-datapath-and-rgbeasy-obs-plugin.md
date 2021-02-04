---

title: "Video Capture with Datapath Cards and RGBEasy OBS Plugin"
categories: [retro, video capture]
excerpt_separator: "<!--more-->"
last_modified_at: 2021-02-02
tags: [retro, gaming, datapath, video capture]
classes: wide

gallery:
    - url: /assets/images/2021-01-29-video-capture-with-datapath-and-rgbeasy-obs-plugin/ps1_gt.png
      image_path: /assets/images/2021-01-29-video-capture-with-datapath-and-rgbeasy-obs-plugin/ps1_gt.png
      title: "Grand Turismo (Playstation) RGB SCART (left) and HDMI via PS1Digital (right)."

gallery2:
    - url: /assets/images/2021-01-29-video-capture-with-datapath-and-rgbeasy-obs-plugin/FBX-OSSC-4x320-Genesis.png
      image_path: /assets/images/2021-01-29-video-capture-with-datapath-and-rgbeasy-obs-plugin/FBX-OSSC-4x320-Genesis.png
      title: "Streets of Rage 2 native resolution (left) and 4x Oversample (right)"

---


## RGBEasy OBS Plugin

When capturing and streaming older video game consoles with Datapath capture cards in OBS Studio, [beeanyew's RGBEasy plugin](https://github.com/beeanyew/datapath-RGBeasy) is an improvement over the default OBS DirectShow implementation as it's easier to configure and has oversampling support.

An alternative to the RGBEasy OBS Plugin is [Tarpeeksi Hyvae's VCS Software](https://github.com/leikareipa/vcs), a standalone application that can control the same RGBEasy API.

The rest of this post will be focused on configuring the Datapath card using beeanyew's OBS Plugin.

## Quick Comparison

Below is the original Playstation with both stock analog RGB video via SCART (left) and lossless digital video with the [PS1Digital](https://www.black-dog.tech/ps1digital.html) mod (right).

{% include gallery caption="Grand Turismo (Playstation) RGB SCART (left) and HDMI via PS1Digital (right)." %}

The RGB source is configured with 4x oversampling using the RGBEasy Plugin and integer/point upscaled in OBS to 1280x960 resolution. The PS1Digital is integer scaled natively to the same resolution, representing a "best-case scenario" of video quality.

Video capture of older consoles through the Datapath E1 is very impressive! At a glance the final result is very close to emulation-level picture quality.

## Hardware

The following is the hardware for the above Playstation RGB video capture:

- [Datapath VisionRGB E1 capture card](https://ebay.to/39EwivC)
- [Insurrection Industries Sony Playstation SCART Video Cable](https://insurrectionindustries.com/product/sega-genesis-model-1-rgb-scart-cable/)
- [SCART2DVI Video Adapter](https://gamesconnection.co.uk/products/scart2dvi?variant=21855284101209)

## Normal Configuration

For game consoles that have a 240p or otherwise non-standard resolution, Datapath cards are **not** a plug-and-play solution for video capture. It requires game-specific configuration in the RGBEasy plugin settings to look correct.

A detailed configuration tutorial is available on the [r3 wiki](https://r3.fyi/Datapath/Capture240p). The **Direct Resolution Method* should be followed for dialing in the correct settings.

The following settings are for Gran Turismo:

|  | Setting |
| --- | --- |
| O. Width | 256 |
| O. Height | 240 |
| H. Size | 341 |
| X Position | 296 |
| Y Position | 35 |
| Phase | 11 |

Some things to note:
- The Output Width, Height, and the H. Size are reliable settings to replicate from setup to setup. The phase setting is setup-specific and requires the tuning mentioned on the r3 wiki.
- Some resolutions on some consoles will require a fractional H. Size value. For example, Playstation games with a 256x240 resolution have an H. Size of 341.25. The best we can do is round to the nearest integer.
- To find the correct H. Size value for a console and game's resolution, [Junkerhq has timings for some popular consoles.](http://junkerhq.net/xrgb/index.php?title=Optimal_timings) **H.samplerate** timings for the OSSC is the same as H. Size for Datapath cards. 
- FirebrandX has a detailed video of how to manually find optimal settings which can be useful if the console and target resolution is yet to be documented. [Watch here.](https://www.youtube.com/watch?v=EBStHr4XCTg)

## Oversampling

Oversampling is an alternative configuration by multiplying the Horizontal Resolution and H. Size by the same constant. It has a few advantages over the normal method of capture. This feature is limited to the RGBEasy API and does **not** work in DirectShow (labeled as Video Capture Device in OBS), making the RGBEasy plugin a requirement.

As an example, here's a comparison of capture settings for Gran Turismo:

|  | Normal | 4x Oversample |
| --- | --- |
| O. Width | 256 | 1024 *(4\*256)* |
| O. Height | 240 | 240 |
| H. Size | 341.25 | 1365 *(4\*341.25)* |

*TODO: Fill in rest of the settings*

The oversampled O. Width and H. Size is the normal sample settings multiplied by the same number (4 in the above example). The O. Width has an upper limit is 2048, so you can use a multiplier as high as allowed.

Finally, the output resolution must be scaled back to a proper 4:3 ratio image. This can be achieved by adding a Scaling/Aspect Ratio filter and bilinear downscale to a 4:3 resolution.

### Why to Oversample

- **Significantly** increases the tolerance for tuning Phase to get the picture right.
- Serves as a workaround for if a game's native resolution requires a fractional H.Size (ex: 427.5 can be multiplied by 2 to be 855 instead).
- Can be a workaround for games that switches between two resolutions by using a oversampled resolution where both in-game O. Widths are multiples of the oversample. This is described in detail on the Genesis page of the [r3 wiki](http://r3.fyi/VGC/GEN)


### A Disadvantage
- Has a slight amount of softness when scrutinized at full resolution.

Here's a comparsion of normal settings and oversampling on a Sega Genesis. Screenshot credit to [FirebrandX.](https://twitter.com/FBXGargoyle)

{% include gallery id="gallery2" caption="Streets of Rage 2 native resolution (left) and 4x Oversample (right)." %}

For video playback or a livestream, the differences between the two are less pronounced as it'll be inevitably compressed. It's worth attempting both configurations to see if the capture looks best with normal settings or with the conveniences afforded by oversampling.
