# Susmita & Rohit - Wedding Invite

4 & 5 December 2026 - Kolkata

One self-contained HTML file. No build step, no npm, no frameworks, no paid services. Open `index.html` and it works. Built fresh - the engagement invite in `../Examples/` was reference only, nothing was inherited from it.

---

## How it works

**The Bengali scene (intro screen only).** Seven animated objects, all CSS and inline SVG, no libraries:

| Object | Motion |
|---|---|
| **Toran** - mango leaves and marigolds on a sagging rope | Each strand sways on its own duration and negative delay, so it never moves as one rigid block |
| **Two diyas** flanking the buttons | Flames flicker on deliberately irregular keyframe stops (11%, 26%, 39%, 54%...) with skew and sideways drift; a smooth pulse is what makes CSS flames look fake. Each pools a warm glow, much stronger in dark mode |
| **Two hanging diyas** in the top corners | Suspended on chains, swinging from the ring on offset durations, flames flickering. Deliberately a different lamp from the seated pair at the bottom |
| **Alpona** | 16 petals draw themselves stroke by stroke on a 0.12s stagger, then turn once every 220s |
| **Ganesha** | Head medallion - ears, mukut, red tilak, curling trunk - drawing itself in on a stagger, with the invocation beneath it |
| **Shankha and pola** | The bride's conch and coral bangles, as a white and red pair of rings around the monogram |
| **Jaba, shiuli, marigold and bel leaves** | Replace the old blob confetti. Tumble in 3D via `rotate3d`. Jaba is the red hibiscus of Bengali puja and carries most of the red in motion; shiuli needs a faint gold outline or it vanishes against cream paper |

Ganesha sits **in the flow**, not absolutely positioned. The intro content is vertically centred, so an absolutely-placed medallion collides with the eyebrow line - and the collision gets worse, not better, on a short or wide screen.

The garad band colour comes from `--band-a` / `--band-b`, defined separately in all four palettes. It cannot be derived from `--accent-d`, because in both dark palettes that variable is a *light* tint, which turned the band into a pale pink strip instead of a deep maroon one.

The toran is **built in JavaScript at the live viewport width**, not drawn into a fixed viewBox. A fixed one stretched with `preserveAspectRatio="none"` scales 3.2× on a 1280px screen and turns every marigold into an ellipse. It rebuilds on resize, debounced.

**Reddish-pink background (intro only).** Six layers under everything else, in `.intro-bg` at `z-index:0`:

| Layer | Motion |
|---|---|
| **3 orbs** | A drifting colour bed, 30-43s each, offset so they never sync |
| **3 pulse rings** | Swell out from behind the monogram and fade, one every 2.5s |
| **Silk sheen** | A diagonal band of light sweeps across every 12s - an occasional event, not a perpetual animation |
| **16 sindoor dust motes** | Drift *upward*, against the falling flowers; the counter-motion is what reads as depth |
| **3 jaba blooms** | Open, hold, fade at fixed spots on a 13s cycle |
| **2 alpona vines** | Draw down the left and right edges over 22s, then erase by running `stroke-dashoffset` on past zero to negative |

Colour comes from `--rose-a` / `--rose-b` (raw `r,g,b` triples, so they can be used inside `rgba()`) plus `--rose-k`, a multiplier that lifts the intensity by 1.5× in both dark palettes where a wash of the same alpha would otherwise disappear.

Every keyframe animates only `transform` and `opacity`, which keeps them on the compositor. The one exception is the vine, which animates `stroke-dashoffset` and so repaints - which is why there are two of them and not ten. Orbs use soft-stopped radial gradients rather than `filter:blur()`, which is expensive on older phones.

Measured over the pink wash rather than the flat background, body text still reads at **11.05:1**, and **9.35:1** on the most heavily tinted paper.

**Intro screen.** The `#SuRo` monogram is itself the button. It has no hard border: a radial halo behind it fades to nothing at the edges, with a thin gold ring that dissolves at left and right. Under it: who is invited, the dates, and the three control toggles. Tapping the monogram runs a 2-second alpona line-draw, then the invitation appears.

**Four looks.** Two palettes and two modes, all four combinations valid:

| | Light | Dark |
|---|---|---|
| **Bengali Wedding** | warm garad ivory, deep alta red `#9d0f18`, temple gold | maroon-black `#190d0e`, gold, soft rose |
| **Premium Mauve** | lilac mist, mauve, champagne | aubergine-black, champagne, lilac |

The Bengali palette carries three ornaments the mauve one does not need:

- **Warm paper.** Two fixed radial washes, gold from the top and alta from the bottom, so the background reads as card stock instead of flat cream. Scoped with `html:not([data-palette="mauve"])`.
- **Garad bands.** The red-and-gold woven border of a Bengali wedding saree, as a 13px strip below the sticky bar and above the closing. Built from stacked repeating-linear-gradients on `--accent` and `--gold-l`, so it retints with the palette rather than being a fixed red.
- **Alpona watermark.** A large 16-petal rosette behind the hero at 8.5% opacity (13% in dark), turning once every 260s.

The names and the closing `#SuRo` are gold foil: a red → gold → red gradient clipped to the text, drifting across on an 11s loop. It sits behind `@supports (background-clip:text)`, because the technique needs `text-fill-color:transparent` and without the guard an unsupporting browser would render the names invisible.

The controls are three symbol medallions - round, hairline gold inner ring, line-art motif, no text. All three are toggles and all three follow the same rule: **the symbol shows what you are switching to, not what you are on.**

| Symbol | Toggles | Shows |
|---|---|---|
| Faceted gem ↔ alpona rosette | Bengali ↔ Mauve palette | Gem in mauve while you are in Bengali; rosette in alta red while you are in Mauve |
| Sun ↔ crescent moon | Light ↔ dark | Moon while you are in light |
| অ ↔ A | English ↔ Bengali | অ while you are in English |

Since there is no visible text, every button carries a `title` and `aria-label`, and all three re-sync whenever the language changes, so the tooltips are themselves bilingual.

Palette and mode are separate toggles, so dark mode works on top of whichever palette is active. **Both persist across a refresh** (`localStorage`). Language does not - every visit starts in English by design, so a guest never lands on a script they cannot read.

**Every switch is gradual, never a snap** - but the palette and the mode get there by opposite means, which is worth knowing before editing either.

- **Palette** (Bengali ↔ Mauve): a `theming` class gives every element a 1.05s eased transition on colour, background, border, shadow, fill and stroke, so the page morphs as one piece. A soft radial wash rises to 40% across the moment of change and clears again. The accent walks `164,22,26` → `152,68,91` → `142,110,150` rather than cutting.
- **Light ↔ Dark**: no wash, because a bright flash on the way into dark mode is exactly what a wash causes. Instead a disc of the *incoming* background colour blooms outward from the centre of the `#SuRo` monogram, soft-edged via a radial mask so there is no hard circle rim. It eases in gently over 1.15s (`cubic-bezier(.5,.05,.3,1)`) so the wavefront is readable rather than swallowing the screen instantly. At 900ms, with the viewport fully covered, the theme is swapped **with all transitions disabled** and the disc then clears, revealing a page that is already finished. 1.85s in total. If no monogram is on screen the bloom starts from the button you pressed.

  Two things this design depends on, both easy to break by accident:

  - The disc's opaque core must genuinely cover the viewport at the swap. A radial-gradient mask with `circle` sizing measures to the *corner*, so an opaque stop at 62% only reaches `0.877 × R`. The radius is set to `1.5 × ` the farthest corner distance to leave headroom. Shrink that and the screen corners sit in the translucent falloff, where the swap becomes visible.
  - The swap happens under `html.instant`, which sets `transition:none` on everything except the disc. Without it, `body` and every text colour ease over ~1s *starting* at the swap, so the background finishes long before the text does and the type visibly trails behind. That was the 1-2 second lag.
- **Language**: the page dissolves to 4% with a 3px blur, the script is swapped while it is invisible, then it reforms. About 1.5s, and the guest never sees the text itself change.

One implementation note worth keeping: the bloom reads its target colour from the `--bg` custom property, **not** from `body`'s computed background. `body` has its own background transition, so reading it right after the swap returns the colour it is animating *from*, and the disc comes out the wrong shade.

All of it collapses to an instant swap under `prefers-reduced-motion`, and a switch already in progress ignores further clicks.

**Section order:** hero → portrait → blessing → our story → film 1 → save the dates → venues → photo pair → the four celebrations → film 2 → gallery → closing.

**Media placement.** Spread through the scroll *and* collected at the end, so nobody can skip past everything:

- Photo 3 as an arch-framed portrait under the names
- Film 1 right after the story ends on "we are getting married"
- Photos 2 and 1 as a pair between the venues and the events
- Film 2 just before the closing
- All five again in the gallery near the end

---

## The videos are silent, permanently

Three layers, so it cannot fail:

1. **The mp4 files have no audio track at all.** Stripped with `ffmpeg -an`. Not muted - absent. There is nothing to unmute.
2. `muted loop playsinline disableremoteplayback`, no controls, and the `muted` property is set in JavaScript too.
3. A `volumechange` listener that forces mute back on if anything ever changes it.

They autoplay when scrolled into view and pause when they leave, so nothing plays off-screen or burns battery. The `<source>` is only attached when a video approaches the viewport, so the page costs almost nothing until then.

---

## Files

```
index.html                  the entire invite
assets/img/photo1-3.jpg     your photos, web-sized
assets/img/venue-qr.svg     QR to the wedding venue (not currently used in the page)
assets/video/film1-2.mp4    your clips, re-encoded and silent
assets/video/*-poster.jpg   still frames shown before autoplay starts
.nojekyll                   stops GitHub Pages running Jekyll
```

Total about 2.9 MB, almost all of it the two videos.

---

## Publish free on GitHub Pages

1. Create a **public** repo, for example `wedding`.
2. Upload the *contents* of this folder to the repo root. `index.html` must sit at the top level, not inside a subfolder.
3. **Settings → Pages → Source: Deploy from a branch**, branch `main`, folder `/ (root)`. Save.
4. A minute later it is live at `https://<your-username>.github.io/<repo-name>/`

```bash
cd "Wedding invite html files"
git init && git add -A && git commit -m "Wedding invite"
git branch -M main
git remote add origin https://github.com/<your-username>/<repo-name>.git
git push -u origin main
```

Live at **https://rohitmukh97.github.io/Wedding_Invite/** from the repo `rohitmukh97/Wedding_Invite`.

### If you ever move it

`og:url` and `og:image` are absolute URLs pointing at that address. WhatsApp and iMessage need absolute URLs for the link preview to show a photo, so if the repo is renamed or moved, update both lines near the top of `index.html` or the preview will silently show no picture.

Send yourself the link and check the preview before sending it to anyone else.

---

## Editing

Every translated string is a pair of sibling spans:

```html
<span class="en">9:00 AM onwards</span><span class="bn">সকাল ৯টা থেকে</span>
```

Change both or the two languages drift apart.

Colours are all CSS variables at the top of the `<style>` block, four sets: `:root` (Bengali light), `[data-theme="dark"]`, `[data-palette="mauve"]`, and `[data-palette="mauve"][data-theme="dark"]`. Change a palette in one place and the whole page follows.

## Replacing the clips later

The current ones came off WhatsApp at 848x478, which is soft, which is why they sit in a contained frame rather than full-bleed. If you get the originals off the phone:

```bash
ffmpeg -i input.mov -an -c:v libx264 -crf 26 -preset slow -pix_fmt yuv420p \
  -movflags +faststart assets/video/film1.mp4
ffmpeg -ss 1 -i input.mov -frames:v 1 -q:v 5 assets/video/film1-poster.jpg
```

Keep `-an`. That is what guarantees silence.
