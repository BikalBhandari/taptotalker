# TapToTalker Context

## Product Goal

TapToTalker is a touch-first communication board for nonverbal and developmentally delayed users. The app should prioritize quick recognition, low cognitive load, and reliable speech output over dense controls or text-heavy interfaces.

The primary user taps visual cards. A caregiver configures vocabulary level and custom card labels/images.

## Design Principles

- Cards should be visually recognizable before reading is required.
- Emoji or custom images are primary cues; labels are supporting cues.
- Each card tap should speak the selected label immediately.
- The final phrase screen should provide only the most important actions.
- Avoid harsh or overly clinical labels when gentler wording works.
- Avoid duplicate UI: selected-word pills already communicate the built phrase.
- Caregiver settings should be available but not dominate the main board.
- Caregiver settings can be protected with an optional local PIN.
- iPad and Android tablet use are the main target surfaces.

## Current UX Decisions

- App name: `TapToTalker`.
- Main flow uses selected-word pills instead of a separate current phrase box.
- Final screen has two large tile actions: `Speak` and `Back home`.
- `Back home` returns to the first screen.
- The discomfort starter uses `Not okay`, not `It hurts`, `Pain`, or `Body`.
- Cards use large visual cues and text labels.
- Some paths naturally end in fewer steps; the step indicator reflects actual path depth.

## Vocabulary Modes

The vocabulary mode controls both how many choices are shown and how deep the flow can go.

- `Simple`: 5 home-screen choices, max 3 steps.
- `Intermediate`: full non-advanced board, max 3 steps.
- `Guided`: capped at 6 choices per screen, max 3 steps.
- `Advanced`: full board plus advanced-only detail cards, max 4 steps.

Advanced-only cards use `minMode: 'advanced'` in the vocabulary tree.

## Card Modes

- `Default`: built-in labels and emoji visual cues.
- `Custom`: uses caregiver-saved labels and uploaded images.
- `Edit`: shows caregiver editing UI.

Customizations are stored in browser `localStorage` using `taptotalker-card-customizations`. The optional caregiver PIN is stored locally using `taptotalker-caregiver-pin`. This means custom cards are local to the browser/device unless a future backend or export/import feature is added.

## Editing Model

In Edit mode, caregivers can switch between editable vocabulary screens using horizontal toggles. The edit list respects the selected vocabulary mode:

- Simple mode shows only Simple-reachable cards.
- Guided mode shows Guided-reachable cards.
- Intermediate mode shows the full non-advanced board.
- Advanced mode shows all cards, including 4-step detail cards.

Caregivers can edit labels and upload custom images. Saving moves card mode to `Custom`.

## Technical Architecture

Single-page Vue app in `src/App.vue`:

- `flow`: nested vocabulary tree.
- `selectedPath`: array of selected option indexes.
- `currentNode`: node derived from `selectedPath`.
- `visibleOptions`: current node options filtered by vocabulary mode.
- `flowStepCount`: actual max depth for current branch, capped by mode.
- `isComplete`: true when selected path reaches the branch depth or no options remain.
- `cardCustomizations`: label/image overrides from localStorage.

Speech uses `window.speechSynthesis` with `SpeechSynthesisUtterance`.

## Known Constraints

- No backend or cross-device sync yet.
- Custom uploaded images are stored as local data URLs, which can consume browser storage.
- The app cannot enforce kiosk lock by itself; use iPad Guided Access or Android App Pinning/kiosk mode.
- Emoji are useful placeholders but real photos may work better for many users.

## Current Project Path

```text
/Users/bbhanda1/Documents/ChatGPT/taptotalker
```
