**English** | [简体中文](README.zh-CN.md)

# ASTERIA-7

Three single-file browser experiments around one fictional spaceship, ASTERIA-7. Each is one `.html`
file: open it in a recent Chrome, Edge, Firefox or Safari and it runs. There is no build step and
nothing to install.

| File | What it is | Libraries | Offline? |
|------|------------|-----------|----------|
| `spaceship-anime.html` | Scroll-through animation showcase: launch sequence, a blueprint that draws itself as you scroll, an orbit with a morphing path, fleet formations, a small pilot/asteroid mini-game, a hyperspace jump | anime.js 4.5.0 (embedded in the file) | Yes |
| `asteria-7-flight-v1.html` | First-person 3D flight demo: cockpit with depth of field, bloom, lens flare, procedural planets, hyperspace jump | three.js 0.186.1 + anime.js 4.5.0 from jsDelivr | No, needs internet |
| `asteria-7-game.html` | The full game: missions, upgrades and seamless landing on real terrain | three.js 0.186.1 + anime.js 4.5.0 from jsDelivr | No, needs internet |

## The game (`asteria-7-game.html`)

You start docked at Halo Station. Take missions from the board, fly them, earn credits and upgrade the ship.

- **Missions:**
  - **Delivery:** land at a spaceport or dock at the station.
  - **Survey:** fly under 5 km inside a zone until the scan completes.
  - **Rescue:** land near a distress beacon, then bring the survivors back.
- **Upgrades:** engine, hull, jump drive and scanner, three levels each.
- **Landable worlds:**
  - Aurelia (ocean)
  - Selene (moon)
  - Vesper (lava)
  - Nivalis (ice)

  Kharon is a gas giant: there is nothing to land on, and diving deep damages the hull.
- **Damage:** a hard impact damages the hull. At zero hull the ship is lost and you respawn at the station with 15% fewer credits.
- **Saving:** progress is saved in the browser's `localStorage`.

### Controls

| Input | Action |
|-------|--------|
| Hold left mouse + drag | Steer (release to re-centre) |
| W / S, mouse wheel | Throttle |
| A / D | Roll |
| R / F | Climb / descend |
| Shift | Boost |
| P | Autopilot to the tracked mission (it stops 1 km above pads) |
| J | Hyperspace jump to the nav target |
| T / N | Next nav target / next tracked mission |
| E | Interact (open the spaceport after landing) |
| C | Cockpit / external view |
| B | Depth-of-field focus toggle |
| X | All stop |
| M | Sound on/off |
| H | Hide HUD |
| Esc | Pause |

Touch devices get an on-screen stick and buttons.

**Landing:** slow below about 30 m/s, keep the ship level and hold **F**. The landing checklist on
the HUD turns green when speed, angle and descent rate are safe. The external view (**C**) makes it
easier to see the ground.

### How the seamless landing works

- Planets seen from far away are spheres coloured in a shader by 3D simplex noise.
- Near a rocky world, three nested terrain patches are built under the ship. They are coarse, medium
  and fine, down to about 100 m per cell. Each one is rebuilt in small time slices so the frame rate
  does not stutter.
- The CPU computes terrain heights from the same noise the GPU uses for colour. The JavaScript port
  rounds every step to float32 (`Math.fround`). Without that, about 40% of sample points disagreed
  with the shader. With it, 256 of 256 matched, so coastlines and mountains line up with the planet
  seen from orbit.
- A logarithmic depth buffer handles metres (the cockpit, the ground) and hundreds of thousands of
  kilometres (the other planets) in one scene.
- Internal resolution adapts automatically to hold about 60 fps.

## Known limits

- The terrain is smooth at very low altitude. There are no trees or buildings.
- It was tested on Windows with Intel UHD 630 integrated graphics: about 60 fps with adaptive
  resolution. It has not been tested on phones or other GPUs.
- The two three.js files load libraries from `cdn.jsdelivr.net`, so they need an internet connection.

## Credits

- [three.js](https://threejs.org/) (MIT) and [anime.js](https://animejs.com/) (MIT, © Julian Garnier). The anime.js
  build embedded in `spaceship-anime.html` keeps its original license header.
- 3D simplex noise by Ashima Arts / Stefan Gustavson (MIT).
- Built with Claude Code.

ASTERIA-7, its planets, companies and all data are fictional.
