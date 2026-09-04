# MTN Find the Badge

A "first click wins" scavenger-hunt game built as a single-page app. Participants
scan a QR code, land on the page, and race to spot a hidden badge shape hidden in
a busy scattered scene. First tap wins; everyone else sees it's been claimed in
real time.

## Firebase setup (do this once, ~5 min)

The shared "who found it first" state lives in a free Firebase Realtime Database.

1. Go to [console.firebase.google.com](https://console.firebase.google.com) and create a new project (no billing needed for this).
2. In the project, click **Build > Realtime Database > Create Database**. Choose any region, and start in **locked mode**.
3. Once created, go to the **Rules** tab and paste this in (restricts read/write to only the paths this game uses):
   ```json
   {
     "rules": {
       "waldo": {
         "winner": {
           ".read": true,
           ".write": true
         },
         "active": {
           ".read": true,
           ".write": true
         }
       }
     }
   }
   ```
   (If you set the database up before the hunt on/off toggle existed, go back to the Rules tab and add the `"active"` block — it isn't there by default.)
4. Go to **Project settings > General**, scroll to "Your apps", click the `</>` (web) icon, register an app (no Firebase Hosting needed), and copy the `firebaseConfig` object it gives you.
5. Open `index.html`, find `FIREBASE_CONFIG` near the top of the `<script>` block, and paste in your `apiKey`, `databaseURL`, and `projectId`.

## Deploying (GitHub Pages)

1. Push this repo to GitHub if you haven't already.
2. In the repo settings, go to **Pages**, set the source to the branch/folder containing `index.html` (root), and save.
3. GitHub gives you a `https://<username>.github.io/<repo>/` URL — that's the link participants will hit.
4. Open that URL, tap the ⚙ icon in the bottom-right, paste that same URL into the QR field, and print/display the generated QR code for the event.

## Files

- `index.html` — the whole game (scene generation, game logic, organizer panel,
  QR code generator, Firebase backend). Single file, no build step.

## Customizing

- `SEED`, `DECOY_COUNT`, `LOGO_X`, `LOGO_Y` near the top of the `<script>` block
  control the scene layout and badge position.
- `LOGO_IMAGE_URL` — once you have the real MTN badge artwork, set this to its
  URL (or a `data:` URI) and it automatically replaces the placeholder hex+star
  shape at the same position, keeping the same tap-forgiveness hit area. Leave
  it empty to keep the placeholder.
- Colors are defined as CSS custom properties at the top of the `<style>` block
  (currently themed MTN yellow/black).

## Organizer controls

Tap the small ⚙ icon in the bottom-right corner of the game screen. The first time
in a browser session, it asks for a passcode (`ADMIN_PASSCODE` near the top of the
`<script>` block — **change it from the default `"mtn2026"` before the event**).
Once unlocked for that browser tab, it stays unlocked until the tab is closed.

From the panel you can:
- Generate/update the QR code for a given URL
- Turn hunting **on/off for everyone** — while off, participants see a "hasn't
  started yet" message and the Start button is disabled (the name field stays
  open so people can get ready); flip it live from anywhere and their screens
  update automatically, no refresh needed. Activating shows everyone a
  synchronized countdown (`COUNTDOWN_SECONDS` near the top of the `<script>`
  block, default 5s) before the Start button unlocks, based on a shared
  server timestamp rather than each phone's own clock
- Reset the round (clears the shared winner in Firebase so you can run another heat)

Note on security: the passcode is a deterrent for casual attendees, not real
authentication — it lives in the page's source, so anyone who views source can
read it. That's an acceptable tradeoff for a single-day event with no backend,
but don't reuse this passcode anywhere sensitive.
