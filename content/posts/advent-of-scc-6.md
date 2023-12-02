---
title: "Advent of Supercomputing: Day 6"
date: 2023-12-06
publishDate: 2023-12-06T08:00:00Z
author: ["yuchen"]
draft: false
---

# `ab-av1`

I play games.  Well, when I'm not destroyed by schoolwork, at least 😭  And sometimes while playing
games, a spectacular game happens and I carry the team with 6 kills!  Time to share that reply with
my friends and save it forever!

> Error: No space left on device  
> -- My SSD

🙃

Apparently recording every match with the fastest encoders on high quality with h264 isn't such a
great idea.  But my laptop is four year old at this point and its performance isn't so good; any
demanding video encoding while playing games is going to tank my frame rate and degrade my
experience.  So, unfortunately, it is out of the question, leaving only re-encoding afterwards as an
option.

Newer video codecs has long been a thing; VP9, a free and patent unencumbered codec,
had been released since ten years ago on and has been widely deployed on YouTube as it lacks the
legal patent issues with h264 and offers higher quality with lower bit rates.  As it stands today,
AV1 is the newest iteration, which offers even [greater quality](https://engineering.fb.com/2018/04/10/video-engineering/av1-beats-x264-and-libvpx-vp9-in-practical-use-case/).  So I definitely want to use AV1 to encode my videos for highest quality with the
smallest file size!

Now that I know what codec I am going to reencode to, time to find the software to reencode it.

## [ffmpeg](https://en.wikipedia.org/wiki/FFmpeg)

ffmpeg is the swiss army tool for multimedia.  Let's use it to encode our video!  Now where is that
manual page...

<https://trac.ffmpeg.org/wiki/Encode/AV1>

Apparently there are multiple encoders for the AV1 codec!  Welp, time to pick that too...

1. libaom
> libaom (libaom-av1) is the reference encoder for the AV1 format. It was also used for research
> during the development of AV1. libaom is based on libvpx and thus shares many of its
> characteristics in terms of features, performance, and usage.
2. SVT-AV1
> SVT-AV1 (libsvtav1) is an encoder originally developed by Intel in collaboration with Netflix.
> In 2020, SVT-AV1 was adopted by AOMedia as the basis for the future development of AV1 as well as
> future codec efforts. The encoder supports a wide range of speed-efficiency tradeoffs and scales
> fairly well across many CPU cores.
3. rav1e
>  librav1e is the Xiph encoder for AV1. Compile with --enable-librav1e. See ​FFmpeg doc and ​upstream CLI options.
4. AMD AMF AV1
> The Advanced Media Framework (AMF) provides developers with optimal access to AMD GPU for
> multimedia processing.

From this description on the ffmpeg webpage, we can immediately see that SVT-AV1 is the future.
It's also said to be the best production encoder right now, so that's what I went with.

## Running SVT-AV1

So now that I've decided on the codec, the encoding software, and the encoder, time to actually
encode the video!

> Much like CRF in x264 and x265, this rate control method tries to ensure that every frame gets the
> number of bits it deserves to achieve a certain (perceptual) quality level.

`ffmpeg -i input.mp4 -c:v libsvtav1 -crf 35 svtav1_test.mp4`

> The valid CRF value range is 0-63, with the default being 50. Lower values correspond to higher
> quality and greater file size. Lossless encoding is currently not supported.

Okay, guess I'm choosing the CRF now.

## Choose the CRF...  Or do we?

Choosing a quality setting is kinda arbitrary.  It depends on your trade off of quality vs size.
But I'm not a video guy.  I don't know how to pick that trade off and wouldn't know the exact point
that the video becomes bad or when quality is excessive and no one can tell a difference.  I can try
testing each CRF and check myself, but it would take a long time and I don't trust myself enough for
that.  If only there's an objective measure of video quality...

## Enter... VMAF!

> Video Multi-method Assessment Fusion, or VMAF for short, is a video quality metric that combines
> human vision modeling with machine learning.  
> -- [Netflix Technology Blog](https://netflixtechblog.com/vmaf-the-journey-continues-44b51ee9ed12)

Now to choose what our VMAF target:

> The bottom line is that if your top rung VMAF score exceeds 95, you’re wasting bandwidth on video
> quality that your viewers won’t notice. For premium content producers, this makes VMAF 95 a good
> target; for UGC and similar content, YouTube and Facebook produce at between 84 – 92.  
> -- [Identifying the Top Rung of a Bitrate Ladder](https://ottverse.com/top-rung-of-encoding-bitrate-ladder-abr-video-streaming/)

Great, since I'm uncompromising on quality, let's go with 95 to be safe.

## But how do I make a video that comes out to 95 VMAF exactly?

ffmpeg doesn't offer this functionality to us, but that doesn't mean that no one wanted to do this.
a couple quick searches landed me on [ab-av1](https://github.com/alexheretic/ab-av1), a tool to do
this calculation automatically:

> AV1 video encoding tool with fast VMAF sampling & automatic encoder crf calculation. Uses ffmpeg,
> svt-av1 & vmaf.

Specifically, we want to use the `auto-encode` subcommand:

> Automatically determine the best crf to deliver the min-vmaf and use it to encode a video or
> image.

This will be done in phases.  First, it cuts a few 20 seconds long clip from the original video.
Then, it uses [interpolated search](https://en.wikipedia.org/wiki/Interpolation_search) followed by
ffmpeg's VMAF to find the CRF that causes these clips to reach the desired target VMAF.  Finally, it
automatically encodes it using the CRF setting that it found.  By default, ab-av1 uses the SVT-AV1
encoder.

Perfect!  Now how do I install and use it?  If you are on Windows, simply download from the GitHub
release page.  If you are on other platforms, install Rust by running the command in
<https://rustup.rs/> and `cargo install --locked ab-av1`.

To use it:

`ab-av1 auto-encode [OPTIONS] -i <INPUT>`

## One final hurdle

Since VMAF is kinda new, most of the ffmpeg distributions doesn't actually include it.  Here's where
I got my VMAF-enabled ffmpeg:

[FFmpeg Static Auto-Builds](https://github.com/BtbN/FFmpeg-Builds/releases/tag/latest)

 ## But who am I to not nerd out the remaining options?

Here's what I came up after spending an entire evening reading the help menu of `ab-av1`:

`ab-av1 auto-encode --min-samples 2 --keyint 2s --scd true`

- `--min-samples 2`: take a minimum of 2 samples, even if the clip is short.  This makes the
  estimate more accurate when the video is too short for ab-av1 to default to 2 samples by taking
  samples in two different time range of the video in case the content changes.
- `--keyint 2s`: put a keyframe every 2 seconds.  This improves seeking time, which is when you go
  forward and backward on a video.  This helps me a lot because I like to use frame by frame seeking
  in mpv to _really_ see when something happens, and that needs to process all the frames starting
  with a keyframe.  Putting keyframes closer together would make its life easier trying to seek.
- `--scd true`:
> Svt-av1 scene change detection, inserts keyframes at scene changes. Defaults on if using default
> keyint & the input duration is over 3m. Otherwise off

  since I decided to change the keyframe interval, I need to reenable this.

I have aliased this in my [`~/.bashrc`](https://git.duckduckwhale.com/DuckDuckWhale/dotfiles/src/branch/main/auto/bash/bash)
so I don't have to remember this:

`alias auto-av1='ab-av1 auto-encode --min-samples 2 --keyint 2s --scd true'`

## Conclusion

Now we have came up with the ultimate video encoding command, next time I can simply `auto-av1 -i
<video>` and it would choose the most efficient parameters for the best encoder to encode in the
best codec to achieve the best video quality without ballooning my video size!
