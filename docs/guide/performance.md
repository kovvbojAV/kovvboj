# Performance

The frame rate is in the top bar. KOVVBOJ aims for 60 fps, and when it drops,
the cause is almost always one of three things: heavy shaders, video
decoding, or network video. Here's how to tell which, and what to do about it.

## Heavy shaders (the usual cause)

Most sets that drop frames are **GPU-bound**. The shaders cost more per frame
than the graphics card has, and the CPU sits mostly idle. Five heavy
generators can take a set from 60 fps to under 20.

- Run **Library → Scan library** before the show, then watch the dots next to
  shader names. A **red** or **orange** shader eats a quarter to more than
  half of a frame on its own. See
  [Effects → How heavy is a shader?](effects.md#how-heavy-is-a-shader)
- Stack **green** shaders freely. Budget the orange and red ones: one or two
  per deck, not five.
- **Mute** layers you're not showing. A muted layer costs nothing, so you can
  keep a big set loaded and only pay for what's on screen.
- Lower the **internal resolution** in **Edit → Settings**. Shaders cost
  roughly in proportion to pixel count, so 1080p costs a quarter of 4K. After
  changing it, run **Library → Rescan everything** so the dots are measured
  again.

## Video clips

Decoding video is the main **CPU** cost. How much it costs depends on the
codec:

| Codec | Cost | Notes |
|---|---|---|
| **HAP** | Lowest | The GPU decodes it directly. Best for VJ clips. |
| H.264 or H.265 at 1080p | Moderate | Decoded in hardware on macOS (VideoToolbox). |
| H.264 or H.265 at 4K | High | A single 4K clip can hold back the whole set. |

- **Convert clips to HAP.** Select a clip layer's source and press **→ HAP**.
  KOVVBOJ transcodes the clip to HAP next to the original, then plays the
  HAP. It uses the FFmpeg bundled with the macOS and Windows packages. On
  Linux, install your distro's `ffmpeg` package.
- **Convert at the size you need.** A 4K HAP clip still moves four times the
  data of a 1080p one. Unless the output really is 4K, make 1080p clips.

## NDI over a slow network

Full-quality 1080p NDI needs about 100 to 125 Mbit/s. A link that can't carry
that doesn't degrade gently. You get a couple of frames a second rather than
a softer picture. On Wi-Fi, or anything slower than gigabit Ethernet, start
KOVVBOJ with:

```sh
RUSTJAY_NDI_LOW_BANDWIDTH=1
```

This receives each NDI source's low-bandwidth proxy stream instead (about
640×360, a few Mbit/s). It applies to every NDI receiver, and KOVVBOJ reads
it once at startup.

## Finding the culprit

- **Mute layers one at a time** and watch the frame rate. The layer that gives
  back the most fps is your problem. If fps jumps but CPU barely moves,
  you're GPU-bound (shaders). If CPU falls a lot but fps barely moves, the
  cost is real but isn't what's limiting you.
- **Log the frame rate.** Start KOVVBOJ from a terminal with
  `RUST_LOG=info,fps=debug` to log it continuously. Judge by the median over
  a run, not the first seconds: startup frames are slow and drag an average
  down.

## Before a show

1. Run **Library → Scan library**.
2. Convert your clips to HAP at the size you need.
3. Mute or remove layers you won't use.
4. Save (**⌘S**), then run the whole set once at show resolution with the
   real outputs connected.
