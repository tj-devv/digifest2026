# Find the Badge

A "first click wins" scavenger-hunt game built as a single-page app. Participants
scan a QR code, land on the page, and race to spot a hidden badge shape hidden in
a busy scattered scene. First tap wins; everyone else sees it's been claimed in
real time.

## ⚠️ Important: where this can actually run

This game uses `window.storage`, a shared key/value persistence API that is
**only available when the page is running inside a published Claude.ai artifact**.
It is not a standard web API — it will not exist on GitHub Pages, Netlify, Vercel,
or any other plain static host.

- **For the live event:** host it via Claude's own published artifact link
  (Publish/Share from within Claude.ai) and point the event QR code at that URL.
  That's the only environment where the "first person to find it wins" logic
  will actually work.
- **This repo:** use it for version control, code review, and future edits —
  not as the deployed source for game night.

If you eventually want a standalone deployment (GitHub Pages, your own domain,
etc.), the storage calls in `index.html` (search for `window.storage`) need to
be swapped for a real backend — e.g. Firebase Realtime Database, Supabase, or a
small serverless function with a KV store. Everything else (the scene, the UI,
the QR generator) is plain HTML/CSS/JS and will work anywhere unchanged.

## Files

- `index.html` — the whole game (scene generation, game logic, organizer panel,
  QR code generator). Single file, no build step.

## Customizing

- `SEED`, `DECOY_COUNT`, `LOGO_X`, `LOGO_Y` near the top of the `<script>` block
  control the scene layout and badge position.
- The badge itself lives in the `#target-logo` group in `buildScene()` — swap in
  your real logo SVG/image there if you have one, keeping the `id`, the
  `transform` (position), and the invisible `.hit-area` circle for mobile tap
  forgiveness.
- Colors are defined as CSS custom properties at the top of the `<style>` block.

## Organizer controls

Tap the small ⚙ icon in the bottom-right corner of the game screen to:
- Generate/update the QR code for a given URL
- Reset the round (clears the shared winner so you can run another heat)
