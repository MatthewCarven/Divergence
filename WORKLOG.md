# Divergence — worklog

## 2026-11-01 — more varied event copy

Matthew wanted the verbs and adjectives randomised more, along the lines of
"SUBJECT FORGETS DURING AN IMPORTANT DECISION".

- Hand-curated word lists rather than a dictionary dump (WordNet etc.): random
  dictionary adjectives break the tone. Everything stays inside `divergence.html`.
- `EMO_VERBS` gained 23 verbs (FORGETS, CHECKS THE TIME, AGREES TOO QUICKLY…), so
  the existing "SUBJECT … IN A STAIRWELL" lines vary more too.
- New lists `MOMENT_ADJ`, `MOMENTS`, `MOMENT_FATES`, plus `occasion()` → "AN IMPORTANT DECISION".
- New templates: `SUBJECT <verb> DURING <occasion>`, `<occasion>, <fate>`,
  `SUBJECT <verb>, THEN <other verb>`, and on the log side `<moment> <fate> — <timing>`.
- Fixed an old bug: `'A ' + pick(NOUNS)` printed "A APOLOGY…", "A INVITATION…" in
  about 2.4% of all lines. New `an()` helper picks A/AN by first letter, so keep
  words like HOUR or UNIQUE out of the lists.
- Checked: 200k generated lines, no article errors, no "X, THEN X" repeats. The
  longest new line is 70 chars (existing max 74). Average line +1.6 chars. Runs in
  the browser with no console errors.
- Build label left at 0.06. Not a git repo yet.

## 2026-11-01 — labels no longer overlap

With two or three labels up at once (RESTLESS tempo, or clicking the ring), they
often drew on top of each other. Slots were per side, so a left label and a right
label could share a height. Narrow rings also pull both sides toward the middle,
and a wrapped block is taller than the gap between slots.

- `drawLabels` now lays labels out in birth order. Each one claims its box, and a
  newcomer that would hit an older one moves to the nearest clear height
  (`collides`, `findRoom`, gap `LABEL_GAP`).
- That offset is worked out while the label is still invisible (before its wipe
  starts) and then kept, so nothing slides on screen. It's stored relative to R, so
  it survives a resize, and is re-checked only if a resize makes it collide.
- Guard: if the window has no size yet (R = 0), no offset is stored. Without that, a
  label born in a 0×0 tab got NaN for its height and never appeared. The in-app
  browser hit this case.
- Tested headless: ran the real page script against a stub canvas, fired 40 bursts
  of three clicks at each of 375×812, 800×600, 1280×720, 1920×1080 and 2560×1440,
  and watched 40 s per burst. Old layout: 31–40 of 40 bursts overlapped at every
  size. New: 0, and no visible label ever moved. Text widths in that harness are
  estimated. A real-browser probe of one three-label burst also measured 0
  overlaps, against 6 colliding line pairs on the old layout.
