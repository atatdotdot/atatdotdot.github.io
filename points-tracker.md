# Two Player Score Tracker

## Overview

This is a lightweight, single-file web application designed to act as a **two-player score tracker** for use on a mobile phone placed between two players. The design prioritises:

* Maximum readability at a glance
* Large, intuitive touch targets
* Symmetry for opposing players
* Zero setup (runs as a single HTML file)
* Offline capability via browser caching and localStorage

The application is intentionally minimal, avoiding external dependencies, build steps, or multiple files.

---

## Purpose

The primary goal is to provide a **frictionless, highly visible scoring tool** suitable for tabletop games, where:

* Two players sit opposite each other
* A single phone is shared between them
* Scores must be updated quickly and accurately
* The interface must be usable without explanation

Key usability requirements:

* Scores are always legible from both sides
* Touch interactions are large and forgiving
* No accidental rapid changes (e.g. from long presses)
* Reset is deliberate and guarded

---

## Architecture

### Single-File Design

The entire application is implemented in a **single HTML file** containing:

* Inline CSS for styling
* Inline JavaScript for logic
* A dynamically generated Web App Manifest

This enables:

* Easy hosting (e.g. GitHub Pages)
* No build tooling
* Simple sharing via URL

---

### Core Components

#### 1. Layout (HTML)

The UI is divided into:

* Two equal halves (`.half.top`, `.half.bottom`)
* Each half contains:

  * Two touch zones (increment/decrement)
  * One score display

A central floating layer contains:

* Reset button
* Confirmation modal

---

#### 2. Styling (CSS)

Key layout techniques:

* `position: fixed` and `100vh / 100dvh` to fill the screen
* CSS Grid (`grid-template-rows: 1fr 1fr`) for equal halves
* `transform: rotate(180deg)` for the top player's view

Design principles:

* **Dark theme** for contrast and reduced glare
* **High contrast typography** using bold sans-serif fonts
* **Large responsive text** via `clamp()`
* **Visual affordances** using translucent overlays

Touch zones:

* Left = decrement (red tint)
* Right = increment (green tint)
* Colour intensity increases on press

---

#### 3. State Management (JavaScript)

State is minimal and consists of:

```js
{
  top: number,
  bottom: number
}
```

Stored in:

* `localStorage` under a versioned key

Behaviour:

* Loaded on startup
* Clamped between 0–99
* Persisted after every change

---

## Interaction Model

### Tap Detection

The app uses **Pointer Events** instead of click events.

A tap is defined as:

* Short duration (< ~420ms)
* Minimal movement (< ~28px)

This ensures:

* Long press does nothing
* Dragging does nothing
* Only intentional taps register

---

### Score Updates

Each touch zone has:

* `data-player` (top/bottom)
* `data-action` (increment/decrement)

On valid tap:

```js
changeScore(player, delta)
```

Where delta is `+1` or `-1`.

---

### Reset Flow

Reset is intentionally guarded:

1. User taps reset button (centre)
2. Modal appears at bottom of screen
3. Confirmation button is spatially separated
4. User must explicitly confirm

This prevents accidental resets during gameplay.

---

## Fullscreen Behaviour

### Problem

Mobile browsers (especially Chrome on Android) do **not allow full removal of UI chrome** in normal tabs.

### Solution

The app behaves like a Progressive Web App (PWA):

* A manifest is generated dynamically
* `display: fullscreen` is specified
* Orientation is locked to portrait

### User Requirement

To achieve true fullscreen:

1. Open the page in Chrome
2. Use "Add to Home screen"
3. Launch from the installed icon

Note: Existing shortcuts must be removed and recreated after updates.

---

## Design Decisions

### 1. Symmetrical Layout

Using rotation instead of duplicating UI ensures:

* Identical behaviour for both players
* Reduced complexity

---

### 2. Large Touch Targets

Each half is split into two **full-height buttons**:

* No small controls
* No precision required

---

### 3. Visual Feedback via Colour

Instead of icons or labels:

* Red = decrement
* Green = increment

This improves speed and clarity under pressure.

---

### 4. Local Storage Persistence

Chosen over cookies or server storage because:

* No network required
* Instant load
* Simplicity

---

### 5. No Long Press Behaviour

Avoids:

* Rapid unintended score changes
* Inconsistent device behaviour

---

### 6. Single File Constraint

Benefits:

* Portability
* Easy hosting
* No dependency management

Trade-offs:

* Manifest must be generated dynamically
* No service worker (optional future enhancement)

---

## Limitations

* Fullscreen requires installation (browser limitation)
* No multi-game history
* No undo functionality
* No animations beyond simple feedback

---

## Possible Enhancements

* Add vibration feedback (if supported)
* Optional sound effects
* Undo last action
* Configurable starting score
* Player naming
* Service worker for offline install improvements

---

## Summary

This application is a deliberately minimal, highly focused tool designed for **clarity, speed, and reliability in a physical, shared context**.

Its architecture reflects that goal:

* Single file
* No dependencies
* Immediate usability
* Strong visual hierarchy

The result is a robust and practical scoring interface optimised for real-world use.
