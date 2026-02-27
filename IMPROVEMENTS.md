# MAKYS – Improvement ideas

Quick wins and next steps to make the app more robust and easier to maintain.

---

## Done in this pass

1. **Fanfare timing** – Fanfare sound now plays 400ms after the result (was `setTimeout(..., -10)`, which ran immediately).
2. **Draw validation** – Draw button checks *before* opening the slot modal: no items left, or “number of draws” > remaining items → alert and no modal/sound.

---

## Suggested next steps

### 1. Shared Firebase config (DRY)

Firebase config is duplicated in **5 files**: `Draw.html`, `PrizeConfig.html`, `PrizeDisplay.html`, `Scanner01.html`, `displayPrize.html`.  

- Add a single `firebase-config.js` that exports `firebaseConfig` and load it with `<script src="firebase-config.js"></script>` in each page.  
- Updating keys or project later = one place to change.

### 2. Firebase SDK version

- Most pages use Firebase JS v9; `Scanner01.html` uses v8.  
- Align on one version (e.g. v9) everywhere to avoid subtle bugs and simplify debugging.

### 3. Sound controls (UX)

- Add a **mute / unmute** (or volume) control on the Draw page so users can turn off slot + fanfare sounds.
- Optionally respect browser/media preferences (e.g. reduce motion or autoplay policies) before playing.

### 4. Error handling and offline

- If Firebase is unreachable or read/write fails, show a short message (e.g. “Can’t sync – check connection”) instead of failing silently.
- Optional: simple retry or “work offline and sync later” for critical flows (e.g. draw results).

### 5. Accessibility

- Give icon-only buttons an `aria-label` (e.g. fullscreen, mute, back).
- Ensure modal focus is trapped while the slot modal is open and returns to the Draw button when closed.
- Keep contrast and focus outlines so keyboard users can see where they are.

### 6. Hub clarity (index)

- You have “Prize Display” (legacy) and “Prize Display v2” (Firebase).  
- Consider renaming to “Prize Display (legacy)” and “Prize Display” (or “Live prize grid”) so it’s clear which one to use at the event.

### 7. Security and deployment

- Firebase client config (including API key) in the repo is normal for Realtime DB, but ensure **Firebase Realtime Database rules** restrict read/write (e.g. by domain or auth) so only your app can access the data.
- If you later use a build step (e.g. Vite/Webpack), move config to env vars and keep keys out of the repo.

### 8. Code structure (optional, larger refactor)

- **Draw.html** is long (script + markup). Splitting the script into e.g. `draw.js` would make it easier to test and maintain.
- Shared visuals (e.g. gradient background, sparkles) are repeated in `Draw.html`, `slotmac.html`, `index.html`. A small shared `common.css` would reduce duplication and keep the look consistent.

---

## Priority order (suggestion)

1. Shared Firebase config + align SDK version.  
2. Mute/volume for sounds.  
3. Basic Firebase error/offline message.  
4. Then accessibility, hub labels, and optional refactors as you have time.

If you tell me which item you want to do first (e.g. “shared Firebase config” or “mute button”), I can walk through the exact code changes step by step.
