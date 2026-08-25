# TapToTalker

TapToTalker is a Vue and Tailwind communication-board prototype for nonverbal users. It lets a user build short spoken phrases by tapping large visual cards.

## What It Does

- Tap-based AAC-style communication flow
- Speaks each selected card using browser speech synthesis
- Builds phrases through 3 steps by default
- Supports 4-step detail flows in Advanced vocabulary mode
- Uses large emoji/image cues plus text labels
- Includes caregiver settings for vocabulary level and card customization
- Supports custom card labels and uploaded images stored locally on the device
- Includes PWA metadata for iPad and Android install-style use

## Run Locally

```bash
npm install
npm run dev -- --port 5174
```

Open:

```text
http://127.0.0.1:5174/
```

Do not open `index.html` directly with `file://`; this is a Vite app and should be run through the dev server.

## Build

```bash
npm run build
```

Preview a production build:

```bash
npm run preview
```

## Vocabulary Modes

- `Simple`: 5 home-screen choices, up to 3 steps
- `Intermediate`: full current board, up to 3 steps
- `Guided`: 6 choices per screen, up to 3 steps
- `Advanced`: full board plus additional detail cards, up to 4 steps

## Card Modes

- `Default`: built-in labels and emoji cues
- `Custom`: caregiver-saved custom labels/images
- `Edit`: caregiver can edit cards and save them as custom

Custom card data is stored in browser `localStorage` under:

```text
taptotalker-card-customizations
```

## Device Notes

For iPad/Android use, run as a web app or install as a PWA where supported. A web app cannot fully lock the user into the app; use platform features for that:

- iPad: Guided Access
- Android: App Pinning or kiosk mode

## Project Structure

```text
src/App.vue                 Main app, vocabulary tree, settings, flow logic
src/main.js                 Vue entry point
src/style.css               Tailwind and global mobile/touch styles
public/manifest.webmanifest PWA metadata
index.html                  App shell and mobile metadata
```
