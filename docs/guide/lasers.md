# Lasers

> **Experimental, and not yet tested with real hardware.** Laser output,
> including the optimiser, the safety thresholds, and the DAC transport, has
> been built and checked on screen, but hasn't yet driven a real laser DAC.
> Show lasers can blind people. Follow your local laser-safety rules, keep a
> hardware interlock or E-stop within reach, and never scan an audience with
> an untested setup.

Laser output needs a build with the `laser` feature. Hardware output needs
`laser-dac`, which pulls in libusb and CMake. The release builds include both.

## How lasers differ from video

A laser doesn't draw pixels. It traces a **path**: a list of points, each with
a position, a colour, and a *shape number* that starts a new stroke. So laser
content never goes through a layer, a chain, or the crossfader. Lasers run as
**laser decks**, a pipeline of their own, alongside the video mixer.

The content is **MadMapper laser materials**, shaders that generate paths
rather than images. About 98 of MadMapper's 109 compile and run.

## The LASERS tab

In STAGE mode, the **LASERS** tab sits next to SURFACES and LIGHTING. From
there:

1. **Material**: pick a laser material (**Browse…**, then **Load**).
2. **Watch the preview.** It shows the path the laser will draw. Tick
   **Show blanked travel** to see the blanked jumps between strokes as well.
   That's where a laser looks wrong first, so check them before you arm
   anything.
3. **Optimiser**: **Retune** adjusts blanking, corner dwell, and stroke
   repeats for the current material. **Skip dark strokes** drops strokes too
   dark to see.
4. **DAC**: **Scan for DACs** finds laser DACs, then **Connect** and
   **Disconnect**.
5. **Arm**: nothing leaves the DAC until the deck is **ARM**ed. **BLACKOUT**
   cuts the output at once. The safety layer also blanks the beam if the scan
   stops moving (scan-fail).

## Aligning with a camera

The **Camera** section fits the laser's output to a region using a camera.
**Start** projects targets and measures where they land, **Solve** computes
the corner-pin, and **Cancel** stops. The output has to be armed first:
calibration won't arm it for you.
