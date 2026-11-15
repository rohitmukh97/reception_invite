# Rohit & Susmita - Wedding Reception Invite

Saturday, 16 January 2027 - 7:00 PM - Twenty Four Hotel, Bangalore
Hosted by the Mukherjee family.

One self-contained HTML file, no build step. Built from the wedding invite in
`~/Downloads/Card invite Creation/Wedding invite html files/` and re-themed.

## #SuRo and back to black and gold (2026-11-15)

A black-and-silver look was tried and dropped; the invite is the original black and gold again
(a copy of the silver version is kept outside this folder). Kept from that round: the medallion
reads #SuRo instead of R ◆ S, and the share card shows #SuRo (`og:image` carries `?v=3`).

## What changed from the wedding invite

- **Two looks, one switch:** black and gold (default, `[data-theme="dark"]`)
  and its exact reverse, gold background with black text and design
  (`:root`). The half-filled circle button, on the opening screen and in the
  top bar, swaps between them with the circular bloom transition; the choice
  is remembered on that device under its own key (`suro-rec-theme`, so the wedding invite on the same github.io address can never make this one open in gold; default is black and gold). Hard-coded colours that cannot come from the
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
  - Our moments (gallery), straight after the hero and before Our journey, in the place of the
    old single portrait (which is no longer shown on the page; portrait.jpg is still used by the PDFs): ten photos (the five from the Kolkata saree-and-kurta shoot,
    two in lavender, the beach sunset and two bike rides), in a mixed order set in the strip markup in arched gold frames on a swipeable strip; tap one
    for a full-screen viewer (swipe or arrows to move, swipe down / Esc / x to close). The
    picture icon in the top bar jumps straight here. Files: `assets/img/gallery/t1-8.jpg`
    (thumbnails, ~50 KB) and `g1-8.jpg` (full, ~200 KB), shrunk from the originals with all
    camera and location data removed; they load only as the guest nears the gallery. To change
    a photo, replace both tN.jpg and gN.jpg with the same names.
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

Doors screen and door transition (the `SMOOTHNESS PASS` CSS block at the
end of the styles): the names on the doors are still foil with no endless
shimmer; the arch and door inlay fade in rather than drawing stroke by
stroke; the doors animate only their rotation; the light flood is one
screen-sized layer centred on the doorway (`--sx` / `--sy`); the top bar is
solid (no backdrop blur); the language switch is a fade, not a blur. About
3 seconds after the doors screen settles, the invitation is quietly made
visible underneath it (`#invite.warm`), so it is already painted when the
doors open, and it no longer fades in on top of the fading doors screen.
The effects canvas caches the screen size so it never forces a layout.
Measured with the CPU slowed 20x: worst frame during the transition went
from 100-220 ms to about 33 ms.

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
- **Journey, more alive (2026-11-15):** a small gold firework pops over each stop as the
  traveller reaches it, and over any stop that is tapped (the stop's label jumps); now and then
  a small firework blooms over the reception stop while the section is on screen; on the
  wedding day (Kolkata) and the reception day (Bangalore) that stop gets a full show instead.
  A tiny heart rises out of the rings and the diamond throws a star glint; fairy lights twinkle
  along the Howrah Bridge cables and a boat drifts across the river; a flag waves on the
  Vidhana Soudha and its windows light one by one. The traveller leaves gold glitter, a
  heartbeat ring pulses where today rests, three lanterns float along the road still ahead,
  and the "in N days" counts roll down to the real number when the section comes into view.
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

## Music

`assets/audio/song.mp3` is Mangal Rang (Orchestral), Ramayana, A.R. Rahman x Hans Zimmer (4.1 MB).
- It is fetched quietly while the guest looks at the doors and starts, fading in, when the
  doors are tapped (phones only allow sound after a tap). It begins at the 6th second and
  loops back to the 6th second (`START_AT` in the script, plus `#t=6` on the audio src).
- It is tried on the very first touch of the doors and, where a phone only allows sound once the
  tap is released, started by the tap itself; either way it is heard as the doors open
  (0.7 s fade-in).
- The speaker button sits with the theme and language buttons, both on the doors screen and in
  the top bar. On the doors screen it only sets the choice. Turning it off is remembered only
  for that visit (`sessionStorage`, `suro-rec-song`), so every new opening of the invite plays.
- While the song plays, a ribbon of gold light moves to the music just under the top bar.
  Opened as a local file (file://) the browser will not let the page listen to the song, so
  there the ribbon breathes on its own while the music plays; on the website it follows the music. It closes and stops drawing when the music is turned off.
- `MUSIC_ENABLED=false` in the script turns all of it off again.

## Files

```
index.html              the entire invite
assets/img/portrait.jpg same photo as the wedding invite (swap any time, same name)
assets/img/venue-qr.svg QR to https://maps.app.goo.gl/Pd8FyUimeRLEw1jY7
assets/img/share-card.jpg link-preview image for WhatsApp / iMessage
assets/audio/song.mp3   the music
assets/img/gallery/     gallery photos (tN thumbnails, gN full size)
.nojekyll               stops GitHub Pages running Jekyll
```

## Before publishing

- `og:url` and `og:image` near the top point to
  `https://rohitmukh97.github.io/reception_invite/`. Change both if the repo
  gets a different name.
- `assets/img/share-card.jpg` (1200x1200, ~106 KB) is the WhatsApp / iMessage
  preview: medallion, names, date and venue in black and gold.
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

## Readability and guest details (2026-11-12)

- Small-caps labels are 11-12px (journey labels and countdown units 10.5px,
  where three columns share the width); the faintest grey is brighter.
- English times say IST (doors screen, hero ribbon, ticket, venue card).
- Venue card: full address and a parking line (dedicated parking zones
  across Prestige Tech Park).
- The formal letter's reception paragraph is shorter; the ticket and venue
  card carry the details.
- Top-bar buttons are 44px; unused font weights are no longer loaded.

## PDF invites (to send on WhatsApp / email)

Two A5 black-and-gold PDFs:
- `Rohit-Susmita-Reception-Invite-English.pdf` (3 pages): cover with the doors, the couple
  (portrait, parents' welcome line, groom with bride), the evening (date, venue, address,
  dinner, parking, QR codes for directions and the online invitation, both also clickable).
- `Rohit-Susmita-Reception-Invite-Bengali.pdf` (4 pages): the same three pages in Bengali, plus
  the নিমন্ত্রণ letter page.

The wording ("following his marriage on 5 December 2026") reads correctly before and after the
wedding. To rebuild after a text change: edit `pdf-source/template.html` (each page is tagged
`data-l="en"` or `data-l="bn"`), then inside `pdf-source` run `python3 build.py`, then
`CH="<path to Chrome>" node topdf.mjs invite-en.html en.pdf` (and the same for `bn`).
The PDFs and `pdf-source` do not need to go to GitHub.
