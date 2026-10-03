# Seuss Space

A low-poly, Dr. Seuss–ish space set: five Cycles stills, and a browser flyer called **ZOOP!** where the ship refuses to keep one silhouette.

## Play

**Live:** https://kalepail.github.io/seuss-space/

Open `game/index.html` in a current Chrome, Edge, or Firefox. Three.js r160.1 (MIT) is vendored at `game/vendor/three.min.js`, so a local copy works from disk with no build step. The only optional network request locally is the Sniglet webfont (the UI falls back to Trebuchet / Comic Sans). The GitHub repo omits that 670KB file. The Pages workflow downloads the same r160.1 build into the site artifact, and the page falls back to jsDelivr only if the file is absent.

Or, from this folder:

```bash
cd game && python3 -m http.server 8765
# http://127.0.0.1:8765/
```

Add `?play=1` to skip the title card.

### Controls

| Input | What it does |
| --- | --- |
| **W / S** | Thrust / brake (you keep a gentle cruise if you let go) |
| **Mouse** (click to capture) or **drag** | Steer |
| **Arrow keys** | Steer without the mouse |
| **A / D** | Roll |
| **Shift** | Zip-boost (wider view, hotter trail) |
| **1 / 2 / 3 / 4** or **F** | Polymorph: Seahorse, Top Hat, Corkscrew, Truffula |
| **Enter** | Fly / fly again |
| **M** | Mute music and effects |

On a phone, drag anywhere to steer and use the **GO / ZIP / MORPH** buttons.

Fly through the mustard rings (streak multiplies the points), collect spinning stars, and don't bonk the rocks or the planets. Three puffs of hull. Score and best are kept in `localStorage` under `seuss-space-best`.

The polymorph is a squash-and-spin crossfade: the old mesh shrinks and twists away while the new one overshoots into place (different topologies, so this is a geometry swap with a lerp, not shared morph targets).

### Sound

Music and effects are synthesized in the page with the Web Audio API. There are no audio files. The theme is a short looping mixolydian oom-pah (bouncy, major, a little ridiculous) scheduled on the audio clock so it stays cheap. It starts on the first **Fly** click, Enter, or tap — not before that gesture. **M** or **Sound off** silences the tune and every effect.

Effects: thrust rumble while **W** / **GO** is held, a boost whoosh on **Shift** / **ZIP**, a rising arpeggio for stars, a chime for rings, a flourish when the ship morphs, a kaboom on a crash, a two-note pulse at one hull puff, and a tick for the title buttons.

## Stills

Cycles CPU renders, 1920×1080, up to 96 samples with adaptive sampling. This Blender build has no OpenImageDenoise, so the stills are raw samples — flat colors clean up well, but a little grain remains in the sky.

| File | Scene |
| --- | --- |
| `renders/01_hero_planets.png` | Striped seahorse in front of a truffula moon, a teal-and-mustard giant, and a polka planet |
| `renders/02_fleet_flyby.png` | Seahorse, top hat, corkscrew, and saucer crossing a banded giant |
| `renders/03_ring_world.png` | Wonky rings around a truffula world, corkscrew threading the gap |
| `renders/04_nebula_canyon.png` | Bent towers and arches, a ship in the lane, a striped planet eclipsing the end |
| `renders/05_puff_comet.png` | Polka nucleus, a tail of puffs, a top-hat ship surfing alongside |

Blender files (one per shot) are in `blend/`. Rebuild them with Blender 4.3:

```bash
blender -b -P blend/build_scenes.py -- preview          # 960×540, 24 samples
blender -b -P blend/build_scenes.py -- full             # 1920×1080, 96 samples
blender -b -P blend/build_scenes.py -- full canyon      # one shot; match png or blend name
```

Geometry stays chunky on purpose: faceted spheres, 5–8 sided tubes, icosahedron puffs and rocks. Color is coral, teal, mustard, cream, and sky blue, with a painted nebula skydome that does not light the scene.

## Limits

- No GPU and no OIDN here, so final stills are 96 adaptive samples rather than a denoised 32.
- The game ships are built in Three.js (toon shading, stripe textures), not exported glTFs — they match the stills' silhouettes rather than sharing the Blender meshes.
- Pointer lock needs a click; arrow keys work immediately.
- The tune is original procedural music, not a recording and not a Dr. Seuss composition.
- The public site is the `game/` folder via GitHub Pages. Blend files and the full-size stills stay local; they are too heavy to publish with the game.
