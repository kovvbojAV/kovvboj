# Control

Beyond the mouse, you can drive KOVVBOJ from a MIDI controller, from other
software over OSC, from a phone or tablet through the web remote, and on a
timeline with the sequencer.

## MIDI

The quickest way to map a controller is **MIDI map mode**. See
[Modulation → MIDI](modulation.md#midi-controller-learn). In short: click
**MIDI** in the top bar, click a control, and move a knob.

**View → MIDI** is where you choose the controller, see every mapping, and
remove mappings. KOVVBOJ remembers the device and your mappings, and
reconnects next time if the device is plugged in.

MIDI mapped to a layer, a deck, or the master keeps working when you reload
the set or recall a saved deck. Bindings follow the thing they're mapped to,
not its position on screen.

## OSC

KOVVBOJ listens for OSC on UDP port **9000** by default. **View → OSC** shows
whether the server is running, lets you change the port, and lists the
parameter addresses. Addresses take the form:

```
/rustjay/<category>/<parameter>   <float>
```

Send a float in the parameter's range.

### Text layers

Set a Text layer's words over OSC. This is useful for song titles, names, and
live captions:

```
/rustjay/text/<layer name>   "<new text>"
```

The layer name is lowercased, with spaces turned into underscores. So a layer
called **Main Title** is `/rustjay/text/main_title`. You can also use the
layer's id.

## Web remote

The **WEB** pill in the top bar shows whether the web server is running.
**View → Web** starts and stops it, and sets the port (**8081** by default).
When it starts, KOVVBOJ logs a URL with an access token, for example
`http://192.168.1.42:8081/…?token=…`. Open it in a browser on any phone or
laptop on the same network to get sliders for every parameter, plus panels
for MIDI and OSC setup, modulation, and presets. The token changes on every
launch.

With the `api` feature (included in the release builds), KOVVBOJ also serves
a REST and WebSocket API with the whole set's structure: decks, layers,
chains, and the library. Live values update as they're modulated. The API
documents itself at `/swagger-ui`, on the same server.

See the engine guide's
[External control](https://bluejaylouche.github.io/rustjay-engine/external-control.html)
chapter for the full protocol.

## Sequencer

**View → Sequencer** runs the crossfader automatically, as a list of steps:

- **+ Crossfade (beats)** and **+ Hold (beats)**: steps measured in beats, so
  they follow the tempo.
- **+ Crossfade (timed)** and **+ Hold (timed)**: steps measured in seconds.

Use **▶ Play**, **⏸ Pause**, and **⏹ Stop**. Tick **Loop** to run the list
over and over. **✖** removes a step.

**Quick Transitions** start a crossfade to a deck straight away:
**Auto → A** and **Auto → B** fade in time, while **Beat-Sync → A** and
**Beat-Sync → B** wait for the beat.

Pressing **TAKE** stops a running sequence.

## Presets

**View → Presets** saves and recalls named snapshots of parameter values.
Presets store parameter values, not the set's structure. To keep whole
layers, decks, and chains, save them to the library instead. See
[Decks and layers → Saving to the library](decks-and-layers.md#saving-to-the-library).
