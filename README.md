# drift

a portable video art player. one html file. no apps, no accounts, no cloud.

---

**drift** turns any phone or laptop into a dedicated video art object. load a folder of clips, swipe or tap through them with smooth crossfade transitions, and lock the device down for installation or exhibition. it's a [progressive web app](https://web.dev/progressive-web-apps/) — runs in the browser, installs to the home screen, works offline.

built for embedding a phone inside a physical enclosure (driftwood, found objects, sculptural housing) to create an intimate, self-contained video piece. also works beautifully as a fullscreen desktop player.

if drift is useful to you, [you can support development on ko-fi](https://ko-fi.com/curiousimagery).

## what it does

- **fullscreen looping video playback** with crossfade, dip-to-black, or hard cut transitions
- **folder-based loading** — point it at a folder, it finds your clips
- **footage banks** — organize clips into switchable groups using filename tags (`[ambient] waves.mp4` or `nature--stream.mp4`)
- **thumbnail grid** with animated preview on the selected cell
- **three input modes**: touch gestures, bluetooth controller (gamepad api), and keyboard
- **quad-tap hidden menu** — settings are invisible until you need them
- **adjustable transition duration** (0.2s – 8s), playback speed, sequential/shuffle order
- **fit/fill scaling** with pinch-to-toggle on touch devices
- **analog scrub** — right stick controls playback speed and direction (opt-in via settings, great for frame-by-frame exploration of slomo, focus stacks, and timelapses)
- **wake lock** to prevent screen sleep
- **guided access compatible** — lock an iphone into a dedicated drift player with no home gesture, no notifications, no escape

## getting started

1. get `drift.html` onto your device (airdrop, email, download, usb — whatever works)
2. open it in safari (ios) or any modern browser (desktop)
3. tap **let's play**, then **add media** and select a folder of video clips
4. tap **launch**

to install as a home screen app on ios: open in safari → share → add to home screen. this gives you a standalone fullscreen app with no browser chrome.

## controls

**touch (iphone/ipad)**

| gesture | action |
|---|---|
| swipe left / right | next / prev clip |
| swipe up | open grid |
| swipe down (grid) | play selected & close |
| tap | show clip info |
| tap cell → tap again | select → play |
| pinch out / in | fill / fit |
| quad-tap | open settings menu |

**bluetooth controller**

| button | action |
|---|---|
| d-pad left / right | prev / next clip |
| d-pad (grid) | navigate cells |
| a | play selected |
| b (player) | open grid |
| b (grid, 1st press) | switch focus to bank tabs |
| b (grid, 2nd press) | return focus to clips |
| d-pad left / right | navigate banks (when bank tabs focused) |
| d-pad down / a | select bank and return to clips |
| start | close grid |
| l / r shoulder | quick-cycle banks (full controllers only) |
| right stick ←→ | scrub speed & direction (enable in settings) |

**keyboard**

| key | action |
|---|---|
| ← → | prev / next (or navigate grid) |
| g / esc | toggle grid |
| enter | play selected |
| b | cycle banks (grid view — press repeatedly to step through) |
| m | open / close menu |
| tab / shift+tab | navigate menu controls (when menu is open) |
| enter / space | activate focused menu button |
| ← → | adjust sliders (when slider is focused) |

## compatible devices & browsers

**tested and working:**
- iphone 14 pro — safari (standalone pwa)
- iphone 17 pro — safari (standalone pwa)
- macbook pro 16" — safari, chrome
- any modern browser with es6+ support

**video format support:**
- `.mp4` (h.264/h.265) — recommended, best compatibility
- `.mov` — works on apple devices
- `.m4v` — works on apple devices
- `.webm` (vp8/vp9) — chrome/firefox, not safari

for best results, export clips as h.264 `.mp4`. keep clips under ~200mb each for smooth blob url handling. resolution is flexible — drift scales to fit or fill the screen.

## kiosk / installation mode

drift is designed to disappear into a physical object. to lock down an iphone:

1. open **settings → accessibility → guided access** and enable it
2. set a passcode (your admin exit code)
3. under guided access settings, set **display auto-lock → never**
4. launch drift from the home screen, load your media
5. triple-click the side button to enter guided access
6. disable hardware buttons, motion, and software keyboard in the options
7. tap **start** — the phone is now a sealed video player

to exit: triple-click → enter passcode → tap **end**.

## architecture

drift is a single html file. all css and javascript are inline. no build step, no dependencies, no node_modules. the only external resource is a google font (dm sans) which caches after first load.

video playback uses two `<video>` elements (`vA` and `vB`) that alternate roles during transitions — one fades up while the other fades down. clips are held as blob urls in memory for the session. a webgl luma blend shader is included but currently shelved (safari compatibility issues).

## file naming for banks

to auto-sort clips into banks, use either format:

```
[ambient] slow waves.mp4
[glitch] broken signal.mp4
nature--flowing stream.mp4
glitch--static burst.mp4
```

clips without tags go into an "all" bank. during playback, switch banks with the b key, shoulder buttons, or bank tabs in the grid.

## known limitations

- **no persistence between sessions** — clips are loaded from the file picker each time. closing the app or force-quitting means re-loading. (indexeddb persistence is a future consideration.)
- **codec support** — browsers can't decode professional editing codecs like ProRes, DNxHR, or Cineform. export clips as h.264 `.mp4` for best compatibility. drift will skip unplayable files during loading and tell you how many were skipped.
- **ios file picker** — `webkitdirectory` shows a standard file multi-select on ios, not a true folder picker. functionally identical, just looks different.
- **memory on older devices** — many large video blob urls may hit memory limits on phones with less than 4gb ram.
- **luma blend transition** — the webgl shader is written but glitchy on safari. shelved until it can be debugged.

## license

[license pending — see discussion on gpl v3 vs cc by-nc vs mit]

## acknowledgments

drift was built entirely through conversation with [claude](https://claude.ai) by anthropic. the concept, design decisions, and every line of code were developed iteratively in dialogue — from initial research into off-the-shelf alternatives, through hardware evaluation, to the final single-file pwa.

---

*drift is a video art object, not a video player. it's meant to be touched, embedded, exhibited, given away. put it inside something beautiful.*
