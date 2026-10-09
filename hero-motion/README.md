# Hero motion: sunrise to sunset on scroll

A scroll-linked colour change for the home-v3 hero (web.sarvam.dev/home-v3).
The orange arc above the headline already reads as a sunrise. As the visitor scrolls, that sky moves through golden hour to dusk. It holds still for a moment, then drifts away with the page.

| Scroll through hero | Phase | What changes |
|---|---|---|
| 0% | **Sunrise** | Nothing. This is today's art, untouched. |
| ~25% | Morning | Core deepens to red-orange; blue halo starts warming. |
| ~50% | **Golden hour** | Peach band, lavender halo; text glow turns peach. |
| ~75–100% | **Dusk** | Rose core, coral band, violet halo; text glow turns lilac. |

"Scroll through hero" means scroll distance as a share of half the hero's height. On a 900px-tall desktop viewport the full day passes in the first 450px of scroll.

Screenshots of the change running on the live page are in [`screenshots/`](./screenshots): desktop at 0/25/50/75/100%, plus a mobile strip.

## Files

```
hero-motion/
├── hero-motion.diff         ← the change
├── before/                  ← what the diff applies to
│   ├── HomeHero.astro       (hero section, reconstructed from the served markup)
│   └── public/assets/pages/home/hero-gradient.svg   (live asset, unchanged)
├── preview/index.html       ← standalone page to scroll and review the motion
└── screenshots/
```

The website source isn't in this repo, so `before/HomeHero.astro` is rebuilt from the HTML that web.sarvam.dev serves. Classes, inline styles and the `BlurButton` / `LogoCarousel` props are copied exactly. The file name and import paths are guesses, so match them to the real hero component when porting.

```sh
cd hero-motion/before && git apply ../hero-motion.diff   # verified to apply cleanly
```

## What the diff does

1. **Sky (three stacked layers).** The existing `hero-gradient.svg` `<img>` moves into a `.hero-sky` wrapper. Two copies sit on top of it: `hero-gradient-golden.svg` and `hero-gradient-dusk.svg`. They have the same shape and blur and differ only in their four gradient stops. Scroll only changes the **opacity** of the top two layers and the **transform** of the wrapper. Both run on the GPU compositor, so the 3000px blurred SVGs are never repainted.
2. **Text glow.** The soft blue glow behind the headline gets two siblings, peach and lilac, which fade in the same way.
3. **Script (~30 lines, no dependencies).** It runs at most once per animation frame and writes three CSS variables on the `<section>`:
   - `--hero-sink`: an ease-out curve for the drift. The sky starts at a standstill and is back to normal scroll speed by the end, so there's no jolt.
   - `--hero-golden` and `--hero-dusk`: smoothstep fades over 0–60% and 40–100% of the range.
4. **Mask.** The bottom 25% of the sky fades out, so the drift never leaves a hard edge on the logo row.

### Palette

| Stop (SVG offset) | Sunrise (current) | Golden hour | Dusk |
|---|---|---|---|
| Core (0.75) | `#F9730C` | `#F45A1E` | `#DC3D5F` |
| Band (0.78) | `#FFB053` | `#FF9466` | `#F4847F` |
| Halo (0.80) | `#A5BBFC` | `#C4B5F4` | `#9E8AE6` |
| Rim (1.0) | `#F4F7FF` | `#FBF5FA` | `#F3F0FF` |
| Text glow | `#A5BBFC → #D5E2FF` | `#FFC4A0 → #FFE6D6` | `#D7A9E6 → #EFDDF6` |

## Behaviour and safety

- **Before the visitor scrolls, the hero looks the same as today.** A pixel comparison of the live hero against the patched one showed no visible difference: 138 pixels on desktop, all text anti-aliasing.
- **No JS / script fails:** all three variables default to `0`, so the hero shows today's sunrise and stays still.
- **`prefers-reduced-motion: reduce`:** the drift is off and the colour change stays. The colour change doesn't move anything on screen.
- **Mobile:** on narrow screens the art sits mostly above the viewport (`top-[-150%] scale-60`), so the visible effect there is the glow behind the headline warming, then turning lilac. If you want the sunset to read more strongly on phones, the place to change is the mobile `top` offset. That changes the resting design, so it's a design decision, not part of this diff.
- **Cost:** two extra SVGs of about 1.2 KB each, one scroll listener (passive, throttled to one update per frame) and a handful of extra GPU layers (the drift wrapper, two sky layers, two glow layers).

## Tuning

| Want | Change |
|---|---|
| Day passes faster or slower | `RANGE` in the script (share of hero height; default `0.5`) |
| More or less drift | `25%` in `.hero-sky__drift`. Keep it at `RANGE / 2` so the drift starts from a standstill, or lower it so the sky scrolls away sooner. |
| When each phase lands | the `smoothstep(from, to, p)` ranges |
| Colours | the four `stop-color`s in each SVG, plus the two glow gradients |
