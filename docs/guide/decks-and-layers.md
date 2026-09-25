# Decks and layers

A set has **two decks, A and B**, and each deck is a stack of **layers**. You
build one deck while the other is live, then cross to it. The two decks are
permanent: every set has them, and they can't be deleted. MIDI mapped to a
deck control keeps working across scene loads.

## Layers

A layer is one picture: a **source**, then its **effect chain**, then opacity
and a blend mode. The top layer of a deck composites over the ones below.

Each layer row, from left to right:

| Control | What it does |
|---|---|
| **≡** | Drag to restack. You can also drag a layer onto the other deck. |
| Thumbnail and name | Click to select the layer. Its settings open in the inspector. |
| **K** | Keying. Turns chroma or luma keying on and off. The key settings are in the inspector, and **K** turns keying back on in the mode you used last. |
| **S** / **M** | Solo and mute. |
| Fader | Opacity. |
| **Norm** ▾ | Blend mode, against the layers below. |
| **✖** | Delete the layer. **⌘Z** brings it back. |

Under the row is the layer's **chain**. It starts with the source chip, then
the layer's effects, then **+**. Click a chip to edit it in the inspector.
Click the dot on an effect chip to bypass that effect. Drag chips to reorder
them, or drag one onto another chain. See [Effects](effects.md).

### Blend modes

There are 15 blend modes. The button shows the short name:

| Short name | Mode | Short name | Mode |
|---|---|---|---|
| Norm | Normal | CBrn | Colour burn |
| Add | Add | Diff | Difference |
| Sub | Subtract | Excl | Exclusion |
| Mult | Multiply | Dark | Darken |
| Scrn | Screen | Lite | Lighten |
| Ovly | Overlay | LBrn | Linear burn |
| SftL | Soft light | HrdL | Hard light |
| CDge | Colour dodge | | |

**Add** and **Scrn** are the quick way to put a bright generator over video.
**Mult** darkens through a texture, and **Diff** gives the classic inverted
overlap.

### Adding layers

- From the library, click **A** or **B** on any row under DEVICES, MEDIA, or
  GENERATORS.
- From a saved layer: saved layers sit under **LAYERS** in the library, with
  their chain and settings intact.

A layer whose file is missing (for example, a clip moved since the set was
saved) stays in the stack, with its chain and settings. Its source chip says
what's missing, so you know which file to put back.

## Groups

A group mixes several layers together and then treats them as one layer, with
its own opacity, solo, mute, and effect chain.

1. Click the first layer, then **⌘-click** (or **Shift**-click) the others to
   add them to the pick.
2. Right-click one of them and choose **Group *n* layers**.

The group header has a drag handle to move the whole group, a collapse arrow,
a **💾** button to save the group to the library, and **✖** to ungroup it. The
layers stay when you ungroup. Right-click a layer inside a group for
**Remove from group** or **Ungroup**. Drag a group's header onto another
group to nest it there.

Saved groups appear under **GROUPS** in the library. **A** and **B** add the
group to that deck.

## Decks

Each deck header has:

- **DECK A** or **DECK B**: click it to select the deck. The inspector then
  edits the deck, and library effects go onto the deck's chain.
- **deck name** and a **💾** button to save the whole deck, with every layer,
  to the library. Saved decks appear under **DECKS**, where **A** or **B**
  *replaces* that deck with the saved one.
- **deck fx**: effects that run over the whole deck, after its layers are
  mixed.
- **S**, **M**, and the fader: deck solo, mute, and opacity.

Keep a few finished decks saved. Recalling one onto the deck that isn't live
is the fastest way to change scene.

## Crossfading

Below the decks:

- The **A** and **B** previews show each deck on its own. With the
  `projection` feature, click a preview to pop that deck out into its own
  window. That's handy on a second screen.
- The **crossfader** blends A into B through the current **transition**. MIDI
  and LFO map modes can drive it like any other control.
- **transition** ▾ chooses how the decks mix, dissolve by default. Click the
  word *transition* to edit its settings in the inspector. See
  [Effects → Transitions](effects.md#transitions).
- **TAKE** crossfades to whichever deck isn't live over the time next to it
  (1.0 s by default, from 0.05 s to 10 s). The shortcut is **⌘T**. The TAKE
  time is a parameter too, so you can map it. TAKE also stops a running
  [sequence](control.md#sequencer).

## Master

Every deck mixes into the **MASTER** row:

- **Dim**: the master dimmer. It's a real engine parameter, so MIDI, OSC, and
  LFOs can all reach it. Map it to a fader for a blackout.
- The **master chain**: effects that run on everything, on the way out. Name
  it and press **Save chain** to keep it. Saved chains appear under
  **CHAINS**, where **+** *replaces* the master chain with the saved one.

## Saving to the library

| What | How | Library group | Library button |
|---|---|---|---|
| A layer | Select it, then **💾 Save to library** in the inspector | LAYERS | **A** / **B** adds it |
| A group | **💾** on the group header | GROUPS | **A** / **B** adds it to that deck |
| A deck | **💾** next to the deck name | DECKS | **A** / **B** replaces that deck |
| The master chain | **Save chain** | CHAINS | **+** replaces the master chain |

Saved items belong to the [workspace](getting-started.md#where-your-work-is-saved).
If you save under a name that's already taken, the new save replaces the
old one. The save button warns you when you hover over it.
