# Rohit & Susmita - Wedding Reception Invite

Saturday, 16 January 2027 - 7:00 PM - Twenty Four Hotel, Bangalore
Hosted by the Mukherjee family.

One self-contained HTML file, no build step. Built from the wedding invite in
`~/Downloads/Card invite Creation/Wedding invite html files/` and re-themed.

## What changed from the wedding invite

- **Two looks, one switch:** black and gold (default, `[data-theme="dark"]`)
  and its exact reverse, gold background with black text and design
  (`:root`). The half-filled circle button, on the opening screen and in the
  top bar, swaps between them with the circular bloom transition; the choice
  is remembered on that device. Hard-coded colours that cannot come from the
  palette (door lacquer, the light behind the doors, the flood, the names'
  foil) have reversed versions in the `the reversed look` CSS block, and the
  falling flowers read `--fl-1..4` from the palette. The browser bar colour
  follows the look. The mauve CSS block is still in the file but unused.
- **Opening screen: the Deco Doors.** Two black doors with gold Art Deco
  inlay under a sunburst arch, Ganesha at the heart of the sunburst, a split
  R | S medallion at the seam and warm light leaking through it. Tapping the
  doors (the `#startBtn` hit area) swings them open in 3D onto "Welcome",
  the light floods the screen, and the invitation is already underneath.
  Below the doors: the Mukherjee family welcome line, the names,
  "Married 05 · 12 · 2026" and the reception date. All styles live in the
  `RECEPTION INTRO: the Deco Doors` block (scoped under `#intro.deco`); the
  door and arch drawings are inline SVG. Timing is in the `startBtn` click
  handler: doors 1.7s, light flood from 1.05s, invite shown at 1.9s, intro
  fades from 2.05s. On short phones the doors shrink, then the lead line
  and the hint drop out.
  The old wedding intro pieces (toran, diyas, curtain veil) are no longer in
  the markup; their CSS and JS are still in the file but do nothing.
- **Inside the invitation (Art Deco, continuing the doors):** all styles are
  in the `RECEPTION INSIDE` CSS block, scoped to `main.dx` with `dx-*` class
  names. The old wedding-page CSS is still in the file but unused.
  - Hero: a round Art Deco sunburst medallion with R ◆ S at its centre
    (the ray ring turns slowly; tap it to whirl it round), gold-foil
    "Rohit & Susmita", a ribbon with "Married 05·12·2026" and
    "Reception 16·01·2027", Bangalore. It replaced a half-circle arch that
    read as a protractor.
  - Portrait in a door-shaped arch, with slowly turning rays behind it and
    a light parallax.
  - I. The couple: one welcome line from the parents, then groom, with,
    bride, inside a stepped-corner frame whose corners draw in.
  - II. The evening: a gold admission ticket (tap to stamp it
    "See you there", tilts under the mouse on desktop), a days / hours /
    minutes countdown to 16 Jan 2027 7 PM IST (a thank-you after the day),
    and Add to Calendar (Google link plus an .ics for Apple / Outlook). The
    calendar entry runs 7 PM to 11 PM IST; change `DTEND` and the Google
    `dates=` value if the end time differs.
  - III. The venue: details, Open in Maps and the QR side by side.
  - IV. The formal letter: a framed card with a #SuRo wax seal that unfolds
    as it scrolls in.
  - Closing: miniature doors that open onto #SuRo (tap to close and open),
    the welcome line and the Mukherjee family sign-off.
  - A thin gold "fuse" under the top bar fills as you scroll.
  - Bengali throughout, with Bengali digits in the countdown and ticket.

## The grand layer

All in the `GRAND LAYER` CSS block and the early `FX` / `GUEST` script, no
libraries:

- **Doors:** a short burst of cold-pyro sparks either side as they open,
  a warm glow, then the invitation fades up. Sparks and fireworks share one
  canvas (`.dx-fx`) that only runs while something is alive. (The
  walk-through zoom and foil flakes were removed for smoothness.)
- **Hero:** an Art Deco chandelier lowers in and gently swings, crystals
  twinkling; two searchlights sweep behind; "Rohit & Susmita" is written in
  letter by letter (Bengali word by word, so conjuncts stay whole) in real
  foil with a sweeping glint and a soft emboss.
- **Our journey (changes with the date):** three stops, Engaged in
  Bangalore (27 Jun 2026, two rings), the wedding in Kolkata (5 Dec 2026,
  Howrah Bridge), the reception in Bangalore (16 Jan 2027, Vidhana Soudha).
  The road already travelled is solid, the rest dotted, and the gold dot
  travels to where today falls on the route and rests there glowing.
  Reached stops are filled gems.
- **Section heads:** a small Art Deco fan opens above each one.
- **Countdown:** a departure-board flip, which spins through numbers the
  first time it comes into view.
- **Ticket:** "Save my ticket" draws the ticket (with the stamp and guest
  name, in the current language and look) as a PNG and opens the phone's
  share sheet, or downloads it where sharing is not available.
- **Closing:** gold fireworks over the little doors when they open.
- **Film grain and vignette** over everything.

## Performance rules (keep these when editing)

Everything that runs continuously animates only `transform` or `opacity`,
so the phone's compositor does the work instead of repainting the page:
the medallion's ray ring, its glow, the chandelier sway, the searchlights
and the portrait rays. The door transition is doors swinging, a 1-second
burst of sparks, a glow fading in and the invitation fading up; there is no
zoom and no full-screen growing layer. The opening screen is switched to
`display:none` once you are through, so its animations stop. The name
glint is a one-shot pass every 9 seconds while the hero is on screen, the
film grain is still, there are 10 falling petals, and the journey
traveller makes three trips then rests.

## Living details (all pause when their section is off screen)

- **Medallion:** the ray ring turns one way and the beaded ring the other;
  R, the diamond and S arrive in turn and the diamond glints every few
  seconds; the four rim jewels catch the light clockwise; a sheen crosses
  the face every ~8 s; a thin ring of light ripples out from the rim.
- **Journey:** stops are small dots (filled once reached); forward chevrons
  sit on the road travelled and the dotted road ahead drifts onward; each
  reached stop pops and its label lights as the traveller passes, leaving a
  short trail of motes; the rings slide together and the stone twinkles,
  water shimmers under Howrah Bridge, a glint crosses the Vidhana Soudha
  dome; today's point breathes softly.
- **Ticket:** slides up with a tilt and settles, then floats gently; a
  holographic gloss sweeps across every ~7 s; the stub nudges on its
  perforation every ~10 s; ADMIT drops in letter by letter and the serial
  number types itself.

Each section gets `data-live`; an IntersectionObserver adds `.live` only
while it is on screen, and CSS pauses every animation inside a section
without it. On very low-power devices (2 CPU cores or 2 GB memory or less)
`html.lite` keeps the arrivals but drops the extra loops.

## Wording that follows the date

The state is worked out in India time in the very first script, before the
page paints, and set on `<html>` as `data-wed` and `data-rec`
(`before` / `today` / `after`), and re-checked every minute so a page
left open moves on by itself. Text with three versions uses
`w-before / w-today / w-after` (wedding) or `r-before / r-today / r-after`
(reception) and CSS shows only the right one.

| | until 4 Dec | 5 Dec | from 6 Dec |
|---|---|---|---|
| Doors screen line | Getting married 05·12·2026 | Getting married today! | Married 05·12·2026 |
| Hero ribbon | Getting married | Married today | Married |
| Journey, Kolkata stop | Getting married, "in N days" | Getting married today! (pulsing) | Married |
| The couple, welcome line | …invite you to the wedding reception of their only son on Saturday, 16 January 2027, in Bangalore, following his marriage on 5 December 2026. | …following his marriage today. | same as before the wedding (BN says the wedding was solemnised) |
| The couple, Bengali heading | পাত্র ও পাত্রী | পাত্র ও পাত্রী | নবদম্পতি |
| Formal letter, first paragraph | …will be married to… (BN …স্থির হয়েছে) | …today… is being married to… (BN আজ…) | …was married to… (BN …সুসম্পন্ন হয়েছে) |

The journey's Bangalore stop reads Reception, then "Reception tonight!" on
16 Jan, then Celebrated. The countdown already switches on its own.

## Guest names in the link

Add `?to=` with the name to the link, spaces as `%20` or `+`:

```
https://rohitmukh97.github.io/reception_invite/?to=Sharma%20Family
```

The doors screen then says "Dear Sharma Family," and the ticket reads
"Admit · Sharma Family" (also on the saved ticket image). Without `?to=` it
says "You & your family". Names are shown as plain text and cut at 48
characters.

## Music (off for now)

In the script, `var MUSIC_ENABLED=false;`. While false, the sound and wave
buttons are hidden and the audio is never requested.

To turn it on:
1. Put the track at `assets/audio/song.mp3`.
2. Set `MUSIC_ENABLED=true`.

## Files

```
index.html              the entire invite
assets/img/portrait.jpg same photo as the wedding invite (swap any time, same name)
assets/img/venue-qr.svg QR to https://maps.app.goo.gl/Pd8FyUimeRLEw1jY7
.nojekyll               stops GitHub Pages running Jekyll
```

## Before publishing

- `og:url` and `og:image` near the top point to
  `https://rohitmukh97.github.io/reception_invite/`. Change both if the repo
  gets a different name.
- There is no `assets/img/share-card.jpg` yet, so a WhatsApp link preview will
  show no picture until one is added (1200x1200 JPEG).
- Scan the QR once with a phone to confirm it opens the hotel.

## Editing text

Every string is an English/Bengali pair:

```html
<span class="en">7:00 PM onwards</span><span class="bn">সন্ধ্যা ৭টা থেকে</span>
```

Change both, or the two languages drift apart.

## Preview locally

```bash
cd "Reception Invite Creation HTML"
python3 -m http.server 8766
# open http://127.0.0.1:8766/
```
