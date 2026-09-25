# Effects

KOVVBOJ's visuals are [ISF](https://isf.video) shaders: small `.fs` files that
describe a picture and the controls that shape it. Over 140 ship with the app,
and you can add as many of your own as you like.

## Generators and effects

The library sorts every shader into one of two groups, based on what it
declares:

- **GENERATORS** make a picture from nothing. **A** or **B** adds one as a new
  layer.
- **EFFECTS** take a picture in. Their ISF declares an `inputImage`. **+** adds
  one to the selected layer, group, or deck, or you can drag it onto any chain.

The **+** on an effect stays disabled until something is selected to put it
on. Click a layer's name, a group header, or **DECK A**/**DECK B** first.

## Chains

Effects live in chains, and a chain runs left to right. There are four places
a chain can be:

| Chain | Runs over |
|---|---|
| A layer's chain | That layer's source, before it's blended into the deck |
| A group's chain | The group's layers, once they're mixed |
| **deck fx** | The whole deck, after its layers are mixed |
| The **master** chain | Everything, on the way out |

To work with the effects in a chain:

- **Select** a chip to show the effect's controls in the inspector.
- **Bypass** an effect with the dot on its chip. The effect stays in place,
  switched off.
- **Reorder** effects by dragging chips within a chain.
- **Move** an effect by dragging its chip onto another chain.
- **Remove** an effect with the **✖** on its chip.

## How heavy is a shader?

Some shaders are nearly free, while others eat half a frame on their own. Run
**Library → Scan library** before a show. KOVVBOJ renders each shader it hasn't
seen yet, measures it, takes a thumbnail, and checks that it compiles. After
that, each row's dot shows its cost:

| Dot | Cost per frame |
|---|---|
| Green | Under 5%. Stack freely. |
| Yellow-green | 5 to 12% |
| Amber | 12 to 25% |
| Orange | A quarter to half a frame |
| Red | Over half a frame on its own |
| Grey | Not analysed yet |
| Dark red | Doesn't compile. Hover over the row for the error. |

The scan never runs on its own, because rendering an unknown shader takes GPU
time away from a live show. Run it between shows. Results are stored per
shader and per machine, so they carry across workspaces and survive renaming
a file. **Rescan everything** measures the whole library again, which you
need to do after changing the internal resolution.

## Transitions

The crossfader mixes the two decks through a **transition**. Nine ship with
the app: dissolve, four wipes, push, iris, zoom, and luma key. Pick one from
the **transition** menu under the decks. Click the word *transition* to edit
its settings (softness, direction, seed, and so on) in the inspector.

Any ISF shader shaped like a transition works. It needs **two image inputs
and a float named `progress`**. Stock ISF transitions and
[gl-transitions](https://gl-transitions.com) ports work unmodified. To add
one, choose **Add transition…** at the top of the transition menu. KOVVBOJ
checks its shape, copies it into your library, and loads it. Any transition
already in a library folder shows up in the list on its own.

## Adding your own shaders

There are three ways to bring shaders in:

- **Add file…** at the bottom of the library. A shader is copied into the
  library and added as a new layer, so it's there next launch too. Videos and
  images are used from where they are.
- **Library → Add folder…** adds a whole folder, including subfolders up to
  four levels deep. KOVVBOJ scans it every launch. Remove a folder under
  **Folders** at the bottom of the library.
- **Drop files into one of your library folders**, then press **↻** at the
  top of the library to rescan.

**Hot reload.** Edit a shader in the library's own shader folder (including
anything you added with **Add file…**) and save it. Any layer using it reloads
straight away, so you can tune a shader with the picture live. Shaders in
folders added with **Add folder…** don't hot-reload.

### What runs

KOVVBOJ's ISF pipeline accepts:

- **Standard ISF**, including multi-pass shaders and persistent buffers.
- **Shaders converted from Shadertoy and glslsandbox**, including the quirks
  that desktop GL drivers tolerate. For example, a `#define` redefined with a
  new value, or a bare `uniform sampler2D`.
- **MadMapper materials**, most of which compile and render.

Some converted Shadertoys read image channels (`iChannel0`, …) through
`IMPORTED` pictures. KOVVBOJ binds those to black, so a shader built around a
photo renders dark until it's edited to read `inputImage` instead.

## Licences

Shaders you find online carry their authors' licences. Shadertoy's default is
CC BY-NC-SA 3.0, which is non-commercial. The bundled shaders and their
licences are listed in [shaders/CREDITS.md](../../shaders/CREDITS.md). Check
that list before you use a shader in paid work.
