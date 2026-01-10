## Table of contents

- [Overview](#overview)
  - [The challenge](#the-challenge)
  - [Screenshot](#screenshot)
  - [Links](#links)
- [My process](#my-process)
  - [Built with](#built-with)
  - [What I learned](#what-i-learned)
  - [Continued development](#continued-development)
  - [Useful resources](#useful-resources)
- [Author](#author)

## Overview

### The challenge

Users should be able to:

- View the optimal layout depending on their device's screen size
- See hover states for interactive elements

### Screenshot

![](./preview.jpg)


### Links

- Solution URL: [https://github.com/keerthana769/Meet-Landing-Page/](Github)
- Live Site URL: [https://keerthana769.github.io/Meet-Landing-Page/](Live Site)

## My process

### Built with

- Semantic HTML5 markup
- CSS custom properties
- Flexbox
- CSS Grid
- Mobile-first workflow

### What I learned

This project helped me strengthen my understanding of **CSS layout mechanics**, **stacking contexts**, and **responsive design patterns**.

---

### 1. `z-index` and stacking order

* `z-index` controls which element appears on top when elements overlap.
* It only works on positioned elements (`relative`, `absolute`, `fixed`, `sticky`).
* Higher `z-index` → closer to the viewer.

```css
.element {
  position: relative;
  z-index: 2;
}
```

* Always pair `z-index` with `position` for predictable results.

---

### 2. Centered layouts vs full-width sections

* A child cannot safely break out of a centered parent using `width` alone.
* Using `100vw` inside constrained layouts causes overflow and scroll issues.
* Semantics (`<main>`) and layout constraints should be separated.
* `<main>` holds content; inner wrappers control width.

---

### 3. Full-width background using pseudo-elements

* `::before` creates a virtual element used to paint full-width backgrounds.
* The content width stays constrained while the background bleeds edge-to-edge.
* Requires absolute positioning and negative viewport margins.

```css
.section {
  position: relative;
}
.section::before {
  content: "";
  position: absolute;
  inset: 0;
  left: 50%;
  right: 50%;
  margin-inline: -50vw;
  z-index: -1;
}
```

* The pseudo-element paints behind without affecting layout.

---

### 4. Why `100vw` causes horizontal overflow

* `100vw` includes scrollbar width.
* This creates extra space on the right and layout shifts.
* `overflow: hidden` hides symptoms, not the cause.
* Use `width: 100%` and adjust padding instead of forcing viewport width.

---

### 5. Cropping images horizontally without changing height

* `object-fit` cannot crop only left/right if height is `auto`.
* The only valid solution is making the image wider than its container and hiding overflow.

```css
.container {
  overflow: hidden;
}

img {
  width: 120%;
  left: 50%;
  transform: translateX(-50%);
}
```

* If height must stay auto, horizontal cropping requires horizontal overflow.

---

### 6. Horizontal centering only (no vertical centering)

```css
img {
  display: block;
  margin-inline: auto;
}
```

---

### 7. Background images vs `<img>`

* Background images default to `background-size: auto`, which causes unexpected cropping.
* Always define sizing and positioning explicitly.

```css
background-size: cover;
background-position: center;
```

* `background-blend-mode` is used to blend color overlays with images.

---

### 8. Reusable full-bleed pattern (need to learn)

Extracted the full-width background logic into a reusable utility:

```css
.full-bleed::before {
  content: "";
  position: absolute;
  inset: 0;
  left: 50%;
  right: 50%;
  margin-inline: -50vw;
  z-index: -1;
}
```

---

### 9. When **not** to use `clamp()`

* Use `clamp()` for fluid sizing, not structural layout changes.
* Media queries are still required for:

  * Layout shifts (grid ↔ flex)
  * Controlled line length
  * Strict design breakpoints

* Use media queries for layout, `clamp()` for typography.

---

### 10. Understanding `clamp()`

```css
font-size: clamp(2.5rem, 5vw, 4rem);
```

* `MIN` → mobile size
* `MAX` → desktop size
* `vw` → smooth scaling in between

Clamp replaces breakpoint-based font sizes, not font roles.

---

### 11. `<picture>` vs layout control

* `<picture>` swaps image sources.
* `<picture>` chooses which image, CSS controls how many images are shown and where.

---

### 12. Full-width backgrounds and horizontal scrollbars

* Using `50vw` / `100vw` inside padded layouts can cause 1–2px overflow.
* This is a known browser rounding issue.

**Correct fix:**

```css
html {
  overflow-x: hidden;
}
```

---

### 13. Why not to center the entire page with Flexbox (try implementing)

* `align-items: center` on `body` is rarely correct for real websites.
* It breaks natural document flow.

**Correct approach:**

```css
body {
  min-height: 100vh;
}
```

* Center content on inner containers instead.

---

### Continued development

I want to deepen my understanding of **CSS fundamentals and layout behavior.**
Since this project was completed under time pressure, I didn’t fully explore every concept by experimenting (adding, removing, and tweaking values). In future projects, I plan to spend more time testing CSS properties in isolation to better understand how and why they work. 

### Useful resources

- [Resource 1](https://www.w3schools.com/html/html5_semantic_elements.asp/) - Helped me understand and apply semantic HTML correctly.
- [Resource 2](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/z-index) - Provided a clear explanation of stacking contexts and how z-index behaves in real layouts.

## Author

- Frontend Mentor - [@keerthana769](https://www.frontendmentor.io/profile/keerthana769)
- LinkedIn - [@keerthana-gurram](https://www.linkedin.com/in/keerthana-gurram/)

