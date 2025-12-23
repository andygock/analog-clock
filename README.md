# `<analog-clock>` Web Component

A reusable, dependency-free **vanilla JavaScript Web Component** that renders an animated analogue clock using SVG.  
It supports smooth sweeping seconds via **browser-native animations**, accurate phase alignment to real time, configurable dimensions and styling, and explicit time synchronisation offsets.

Licensed under **MIT License**.

---

## Features

- Pure vanilla JS. No frameworks, no build step.
- SVG rendering with crisp scaling at any size.
- **Smooth sweeping second hand** using browser animation engine (not JS timers).
- Accurate alignment to minute and second markers.
- Configurable appearance via HTML attributes.
- Proportionate defaults derived from clock size.
- Supports:
  - Manual time setting
  - Time offsets
  - Timezone offsets
  - Pause and resume
- Works in Shadow DOM and normal DOM contexts.

---

## Installation

Copy `analog-clock.js` into your project and load it as a module or classic script.

### Classic script

```html
<script src="analog-clock.js"></script>
```

You could also load it via jsDelivr with:

```html
<script src="https://cdn.jsdelivr.net/gh/andygock/analog-clock/analog-clock.js"></script>
```

### ES module

```html
<script type="module" src="analog-clock.js"></script>
```

The component registers itself as `<analog-clock>`.

---

## Basic Usage

```html
<analog-clock></analog-clock>
```

This renders a clock with sensible defaults and a smooth sweeping second hand.

### Set the size

```html
<analog-clock size="240"></analog-clock>
```

Size is in pixels and controls all proportional defaults.

### Set other style attributes

```html
<analog-clock
  size="180"
  face-color="#f5f5dc"
  rim-color="#8b4513"
  marker-color="#2f4f4f"
  hour-hand-color="#daa520"
  minute-hand-color="#daa520"
  second-hand-color="#dc143c">
</analog-clock>
```

You can see the examples above on [this live demo at JSBin](https://jsbin.com/pepewodigi/edit?html,output).

---

## Attributes

### Core

| Attribute        | Description              | Default |
| ---------------- | ------------------------ | ------- |
| `size`           | Clock diameter in pixels | `180`   |
| `paused`         | Pause animation          | `false` |

Smooth seconds are always enabled; there is no attribute to disable the sweep.
The clock defaults to your browser's local timezone (same as calling `new Date().getTimezoneOffset()`), but you can force a different offset with `timezone-offset-minutes` if needed.

---

### Time Control

| Attribute                 | Description                                  |
| ------------------------- | -------------------------------------------- |
| `tick-offset-ms`          | Millisecond offset applied to displayed time |
| `timezone-offset-minutes` | Fixed timezone offset relative to UTC        |

Offsets are applied **before phase correction**, so alignment remains exact.

---

### Colours

| Attribute             | Description                 |
| --------------------- | --------------------------- |
| `face-color`          | Clock face fill             |
| `rim-color`           | Outer rim stroke            |
| `marker-color`        | 5-minute markers            |
| `minute-marker-color` | 1-minute markers (optional) |
| `hour-hand-color`     | Hour hand                   |
| `minute-hand-color`   | Minute hand                 |
| `second-hand-color`   | Second hand                 |
| `centre-cap-color`    | Centre cap                  |

If `minute-marker-color` is not set, minute markers are automatically dimmed.

---

### Dimensions (optional overrides)

All of these have **automatic proportionate defaults** based on `size`.

| Attribute              | Description        |
| ---------------------- | ------------------ |
| `rim-width`            |                    |
| `marker-width`         |                    |
| `marker-length`        |                    |
| `minute-marker-width`  |                    |
| `minute-marker-length` |                    |
| `hour-hand-width`      |                    |
| `minute-hand-width`    |                    |
| `second-hand-width`    |                    |
| `hour-hand-length`     | Fraction of radius |
| `minute-hand-length`   | Fraction of radius |
| `second-hand-length`   | Fraction of radius |
| `centre-cap-radius`    |                    |

If not specified, defaults are computed to look visually balanced.

---

## JavaScript API

The component exposes methods for precise time control.

### Sync to system time

```js
const clock = document.querySelector("analog-clock");
clock.syncToSystemTime();
```

### Set explicit epoch time

```js
clock.setEpochMs(Date.now() + 5000); // 5 seconds ahead
```

### Apply incremental offset

```js
clock.adjustByMs(250); // advance by 250 ms
```

### Set fixed timezone offset

```js
clock.setTimezoneOffsetMinutes(600); // UTC+10
```

### Pause / Resume

```js
clock.pause();
clock.resume();
```

---

## How Smooth Seconds Work

Smooth motion is always enabled so the second hand never ticks; it is powered by the **Web Animations API** instead of JS timers.

* The animation runs continuously with a 60-second linear rotation.
* Every 250 ms the animation is **phase-corrected** using:

  ```
  (epoch + offsets) mod period
  ```
* This ensures:

  * Perfect alignment at second boundaries
  * Smooth motion between boundaries
  * Correct behaviour even if system time changes

This approach avoids jitter and timer quantisation issues common with `setInterval` or `requestAnimationFrame`.

---

## Browser Support

* Modern Chromium, Firefox, Safari
* Requires SVG + Web Animations API
* No polyfills required for current browsers

---

## Notes

* Designed to be embedded multiple times on the same page.
* No global variables or side effects.
* Safe for Shadow DOM encapsulation.
* Styling is controlled entirely via attributes.

