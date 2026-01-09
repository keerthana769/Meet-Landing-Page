# Frontend Mentor - Meet landing page solution

This is a solution to the [Meet landing page challenge on Frontend Mentor](https://www.frontendmentor.io/challenges/meet-landing-page-rbTDS6OUR). Frontend Mentor challenges help you improve your coding skills by building realistic projects. 

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

- Solution URL: [Add solution URL here](https://your-solution-url.com)
- Live Site URL: [Add live site URL here](https://your-live-site-url.com)

## My process

### Built with

- Semantic HTML5 markup
- CSS custom properties
- Flexbox
- CSS Grid
- Mobile-first workflow

### What I learned

Use this section to recap over some of your major learnings while working through this project. Writing these out and providing code samples of areas you want to highlight is a great way to reinforce your own knowledge.



### Continued development

Use this section to outline areas that you want to continue focusing on in future projects. These could be concepts you're still not completely comfortable with or techniques you found useful that you want to refine and perfect.



### Useful resources

- [Resource 1](https://www.w3schools.com/html/html5_semantic_elements.asp/) - This helped me for writing semantic html.
- [Resource 2](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference/Properties/z-index) - This is an amazing article which helped me with knowing z index .


## Author

- Frontend Mentor - [@keerthana769](https://www.frontendmentor.io/profile/keerthana769)
- LinkedIn - [@keerthana-gurram](https://www.linkedin.com/in/keerthana-gurram/)




Your .experience-more section has:

background-color: var(--color-cyan-600);


And your .numbers element:

Comes before .experience-more in the DOM

Has no z-index

Is being visually covered by the next element’s background

👉 Backgrounds paint over previous siblings

So even though the circle is physically there, it’s under the blue background.

That’s why:

It “occupies space”

But you can’t see it

✅ The FIX (minimal & correct)
Step 1: Give the number stacking priority
.numbers {
  position: relative;
  z-index: 2;
}

Step 2: Ensure the blue section stays below
.experience-more {
  position: relative;
  z-index: 1;
}


That’s it.
No grid. No restructure.







z-index (Simple Explanation)

z-index controls which element appears on top when elements overlap on a page.

Think of elements as layers:

Higher z-index → closer to the viewer

Lower z-index → stays behind

Basic rule

z-index works only when elements overlap.

.element-a {
  position: relative;
  z-index: 2;
}

.element-b {
  position: relative;
  z-index: 1;
}


In this case, .element-a appears above .element-b.

Important requirement

For predictable behavior, the element should have a position value:

position: relative | absolute | fixed | sticky


Without positioning, z-index may appear to work but is not reliable.

Key takeaway

Use z-index to control stacking order when elements overlap, and always pair it with position for consistent results.








❌ Problem

Even though it’s a separate section:

It is still a child of <main>

<main> is centered and constrained

100vw ignores the parent and uses viewport width

The browser then recalculates layout, causing:

Horizontal overflow

Other content appearing full width

Scroll issues

👉 The issue is not the section — it’s where it lives

🧠 KEY RULE (memorize this)

A child cannot safely “break out” of a centered parent using width alone.

This is why the problem persists.









3️⃣ Why this is the correct mental model

Semantics describe meaning.
Layout describes presentation.
They should not be the same element.

Once you separate them:

Full-width sections are easy

Centered content is easy

No hacks (100vw, negative margins, etc.)


5️⃣ One-line README explanation (you can include this)

The <main> element contains all primary content, while layout constraints are handled by inner wrapper elements to allow both constrained and full-width sections.






















Great question — this is **exactly the right place to pause and understand**, because this pattern looks like “CSS magic” until it *clicks*.
I’ll explain it **line by line**, **with a mental picture**, and **why each part exists**.

No assumptions.

---

## The big idea (1 sentence)

> This code creates an **invisible box behind the section** and stretches that box to the **full width of the screen**, without changing the section’s own layout width.

Keep that in mind as we go.

---

## 1️⃣ What `::before` actually is

```css
.experience-more::before
```

This creates a **virtual element**:

* It’s **not in your HTML**
* It behaves like a real element
* It lives **inside `.experience-more`**
* It is drawn **behind its content**

Think of it as a **paint layer**.

---

## 2️⃣ Why `content: ""` is required

```css
content: "";
```

Without this:

* The pseudo-element **does not exist**
* Nothing is rendered

Empty string = “create the element, but don’t show text”.

---

## 3️⃣ Why `position: absolute`

```css
position: absolute;
```

This allows the pseudo-element to:

* Be **freely sized**
* Ignore normal document flow
* Sit exactly where we want

Because `.experience-more` has:

```css
position: relative;
```

👉 The pseudo-element is positioned **relative to `.experience-more`**, not the page.

---

## 4️⃣ What `top: 0; bottom: 0;` does

```css
top: 0;
bottom: 0;
```

This means:

> “Stretch vertically from the top edge of the section to the bottom edge.”

So now the pseudo-element:

* Has the **same height** as `.experience-more`

No guessing heights.

---

## 5️⃣ The confusing part (this is the key)

```css
left: 50%;
right: 50%;
margin-left: -50vw;
margin-right: -50vw;
```

Let’s break this slowly.

---

### Step 1: `left: 50%; right: 50%`

This collapses the element into the **horizontal center** of its parent.

Visually:

```
|     |  ← left 50%   right 50%
```

The element now has **zero width**, centered.

---

### Step 2: `margin-left: -50vw; margin-right: -50vw`

Now we say:

> “Expand this zero-width element **half the viewport width to the left** and **half to the right**.”

So it becomes:

```
|---------------------------|
        full viewport width
```

Important:

* `vw` = viewport width
* Not parent width
* Not content width
* Actual screen width

---

## 6️⃣ Why this does NOT break your layout

Because:

* The pseudo-element is **absolutely positioned**
* It does NOT affect siblings
* It does NOT affect parent width
* It only paints behind

Your real section stays centered and constrained.

---

## 7️⃣ Why `z-index: -1` is needed

```css
z-index: -1;
```

This pushes the pseudo-element:

* Behind the section content
* But still inside the section stacking context

Without this:

* It would sit **on top of your text**

---

## 8️⃣ Final mental picture (THIS is the click moment)

Imagine `.experience-more` like this:

```
┌───────────── content box ─────────────┐
│   text, button, etc                   │
│                                       │
└───────────────────────────────────────┘
```

Now `::before` becomes:

```
┌──────────────────────────────────────────────┐
│                background                    │
└──────────────────────────────────────────────┘
```

Same height
Much wider
Behind content

---

## 9️⃣ Why this pattern is used in real projects

This exact trick is used when:

* Page content is centered
* Background must bleed edge-to-edge
* Semantics must remain clean
* No extra markup is allowed

Used in:

* Landing pages
* CMS-driven sites
* Design systems
* Frontend Mentor solutions

---

## 10️⃣ One-line explanation (README-friendly)

> A pseudo-element is used to paint a full-width background behind the section while keeping the section’s content width constrained.

---

## Final reassurance

This is **not a hack**.
It’s a **controlled, intentional layout technique**.

Once you understand this, you’ll start noticing it *everywhere*.

If you want:

* A diagram version
* A reduced beginner version
* Or an animation demo explanation

Just tell me 👍












Why your current code behaves weirdly

You wrote:

picture {
  width: 100vw;
}

.top-header {
  overflow: hidden;
}


And you see:

space only on the right side

content looking like it has padding / overflow

🔴 The real reason (this is the key)

100vw includes the browser scrollbar width.

So:

100vw is slightly wider than the visible page

The extra width spills to the right

overflow: hidden hides the overflow, but after layout

Result: everything looks shifted

This is a classic 100vw problem — not a mistake by you.

❌ Why this approach will NEVER be stable

Using 100vw:

fights against centered layouts

breaks when scrollbars appear/disappear

behaves differently across browsers

causes exactly the “right-side gap” you’re seeing

So let’s stop fighting the browser.

✅ The SIMPLEST, CORRECT solution (no hacks, no vw)
👉 Do NOT force full width

Instead:

Let the image be width: 100%

Remove horizontal padding only for the hero

Let cropping happen naturally



















Got it. This constraint changes everything — and now I can give you a **clear, honest answer**.

> **If you don’t want to touch height at all**, then `object-fit` **cannot** do what you want by itself.

This is **not you**, this is **how CSS works**.

---

## 🚫 First, the hard truth (important)

**Cropping only left & right requires horizontal overflow.**

Horizontal overflow can be created **only if one of these is true**:

1. The container is **shorter** than the image (you said ❌ no height changes)
2. The image is **wider than the container**

You removed option #1.
So **only option #2 remains**.

👉 That means: **you must intentionally make the image wider than its container**

There is **no CSS property** that says

> “crop left & right but don’t change height and don’t resize container”

It doesn’t exist.

---

## ✅ The ONLY way to crop left & right **without touching height**

### 👉 Make the image wider than the container

This is the missing piece.

---

## ✅ Minimal, WORKING solution (no height change)

```css
.hero-media {
  width: 100%;
  overflow: hidden;
}

.profiles {
  width: 120%;              /* 👈 wider than container */
  max-width: none;
  height: auto;

  position: relative;
  left: 50%;
  transform: translateX(-50%);
  display: block;
}
```

### What this does

* Image is **120% wide**
* Container shows only the middle
* Left & right overflow is hidden
* Height stays natural
* No `object-fit`
* No height changes
* No `100vw`

✅ Exactly what you asked for

---

## 🧠 Why this works (simple explanation)

Think of it like this:

```
[  IMAGE IMAGE IMAGE IMAGE IMAGE  ]   ← image (120%)
      [  visible window  ]            ← container (100%)
```

You’re not “cropping” the image —
you’re **showing only the center slice**.

This is the **only mathematical way** to crop horizontally without changing height.

---

## ❌ Why `object-fit` cannot help here

`object-fit`:

* only works when **both width & height are controlled**
* needs a fixed container box
* cannot crop in one direction selectively

So with:

```css
height: auto;
```

`object-fit` becomes almost irrelevant.

---

## ✅ If you want *more* or *less* crop

Just change one value:

```css
width: 110%;  /* subtle crop */
width: 130%;  /* heavy crop */
```

No media queries needed.

---

## 🚨 Important note about Figma

Figma often:

* scales images visually
* crops without explicit height
* hides this math from you

On the web, **you must choose**:

* control height
* OR control width

You chose width — this is the correct CSS response.

---

## TL;DR (save this)

> If height must remain auto, the only way to crop left & right is to make the image wider than its container and hide overflow.

Anything else is physically impossible in CSS.

---

## You weren’t failing

You hit a **real limitation of the platform**, not a skill issue.

You asked the exact right question:

> “Can I do this without touching height?”

Now you know the exact boundary — and the correct workaround.

If you want, next I can:

* Match this exactly to the Figma look
* Tune it for tablet/desktop
* Explain when this approach is better than `object-fit`

Just say 👍










✅ Method 3: Horizontal center only (very common)

Use when vertical centering is not needed.

img {
  display: block;
  margin-inline: auto;
}













Great, this is a **very good question** — you’re extremely close.
What you’re seeing (the image looking *zoomed in*) is happening for **clear, fixable reasons**.

Let’s break this into **what’s happening**, **why**, and **the exact fix**.

---

## 1️⃣ What your current code is doing

```css
.experience-more::before {
  content: "";
  position: absolute;
  top: 0;
  bottom: 0;

  left: 50%;
  right: 50%;
  margin-left: -50vw;
  margin-right: -50vw;

  background-color: var(--color-cyan-600);
  background-image: url("./assets/mobile/image-footer.jpg");
  background-repeat: no-repeat;
  z-index: -1;
}
```

### Key points:

* The pseudo-element spans **full viewport width**
* It has **both a background color and an image**
* BUT:

  * You did **not define how the image should size**
  * You did **not define how color + image should interact**

So the browser:

* Defaults `background-size` to `auto`
* Centers the image
* The image appears **zoomed / cropped unpredictably**

---

## 2️⃣ Why the image looks “zoomed in”

Because **background images do NOT behave like `<img>`**.

Default behavior:

```css
background-size: auto;
```

This means:

* The image keeps its original size
* The container is larger
* Only part of the image is visible
* It *looks* zoomed even though it’s not

👉 This is expected behavior.

---

## 3️⃣ How to make the image occupy the full background correctly

You need to explicitly tell the browser **how the image should fit**.

### ✅ Use `background-size: cover`

```css
background-size: cover;
background-position: center;
```

This means:

* Image fills the entire background
* Aspect ratio preserved
* Extra parts are cropped evenly

---

## 4️⃣ How to blend background color and image (THIS is the missing part)

CSS supports **background blending**.

### ✅ Use `background-blend-mode`

```css
background-blend-mode: overlay;
```

Other useful values:

* `multiply` (darker)
* `overlay` (balanced)
* `soft-light` (subtle)
* `darken`

---

## 5️⃣ ✅ FINAL CORRECT VERSION (copy-paste)

```css
.experience-more::before {
  content: "";
  position: absolute;
  inset: 0;

  left: 50%;
  right: 50%;
  margin-left: -50vw;
  margin-right: -50vw;

  background-color: var(--color-cyan-600);
  background-image: url("./assets/mobile/image-footer.jpg");

  background-size: cover;          /* 👈 important */
  background-position: center;     /* 👈 important */
  background-repeat: no-repeat;

  background-blend-mode: overlay;  /* 👈 blending */

  z-index: -1;
}
```

---

## 6️⃣ If the image still feels “too strong”

Lower the image intensity instead of resizing it:

### Option A: Softer blend

```css
background-blend-mode: soft-light;
```

### Option B: Add transparency to color

```css
background-color: rgba(77, 150, 169, 0.85);
```

This is **very common in hero/footer sections**.

---

## 7️⃣ Mental model (remember this)

> **`background-size` controls scale**
> **`background-blend-mode` controls mixing**
> **Without both, results look wrong**

---

## 8️⃣ Why this is better than other approaches

✅ No extra markup
✅ No `<img>` layout issues
✅ Perfect for decorative sections
✅ Matches Figma overlays exactly

This is the **right tool for footer / CTA backgrounds**.

---

## TL;DR

Your image looked zoomed because:

* `background-size` was missing
  Your color didn’t blend because:
* `background-blend-mode` was missing

Adding **`cover + blend-mode`** fixes both.

---

If you want next:

* Match **exact Figma blend**
* Adjust for tablet/desktop images
* Choose best blend mode visually

Just tell me 👍










6️⃣ Repeated full-width background trick → reusable pattern

You used this pattern once, but if reused later, extract it:

.full-bleed::before {
  content: "";
  position: absolute;
  inset: 0;
  left: 50%;
  right: 50%;
  margin-left: -50vw;
  margin-right: -50vw;
  z-index: -1;
}


Then:

.footer::before {
  composes: full-bleed;
  background-color: var(--color-cyan-600);
  background-image: url(...);
}











6️⃣ When NOT to use clamp()

Still use media queries when:

Layout changes (grid → flex)

Line length needs control

Design calls for strict jumps

👉 Use clamp() for size, not structure.


8️⃣ One-line README explanation

Typography uses CSS clamp() to scale fluidly across screen sizes without relying on multiple media queries.

Final mentor takeaway 🧠

Use media queries to change layout.
Use clamp() to scale typography.









font-size: clamp(2.5rem, 1.9718rem + 2.2535vw, 4rem);













Great — this is a **very concrete conversion task**, so I’ll do it **step by step**, mapping **exactly what you already have** into `clamp()` without changing your design intent.

No theory fluff.

---

## 1️⃣ First: understand what you currently have

You’ve defined typography **by device**:

* Mobile
* Tablet
* Desktop

Example for **H1**:

```css
mobile:  40px
tablet:  48px
desktop: 64px
```

That is **perfect input data** for `clamp()`.

---

## 2️⃣ The clamp formula we’ll use (simple & safe)

```css
clamp(MIN, FLUID, MAX)
```

Where:

* **MIN** = mobile size
* **MAX** = desktop size
* **FLUID** = something that grows smoothly (usually `vw`)

We’ll **not over-engineer** the math — readable > clever.

---

## 3️⃣ Convert each text role (one by one)

### 🔹 Text 1 (Main heading / H1)

From your tokens:

* Mobile: `40px`
* Tablet: `48px`
* Desktop: `64px`

✅ Clamp version:

```css
--text-1: 900 clamp(2.5rem, 5vw, 4rem) / 110% "Red Hat Display", sans-serif;
```

Explanation:

* `2.5rem` = 40px
* `4rem` = 64px
* `5vw` gives smooth growth through tablet

---

### 🔹 Text 2 (Section heading / H2)

From your tokens:

* Mobile: `32px`
* Tablet: `36px`
* Desktop: `40px`

```css
--text-2: 900 clamp(2rem, 3.5vw, 2.5rem) / 110% "Red Hat Display", sans-serif;
```

---

### 🔹 Text 3 (Eyebrow / small uppercase text)

From your tokens:

* Mobile: `12px`
* Tablet: `14px`
* Desktop: `16px`

```css
--text-3: 400 clamp(0.75rem, 1.5vw, 1rem) / 110% "Red Hat Display", sans-serif;
```

(Your letter-spacing stays separate 👍)

---

### 🔹 Text 4 (Body / paragraph text)

From your tokens:

* Mobile: `16px`
* Tablet: `16px`
* Desktop: `18px`

Since mobile & tablet are same, keep it subtle:

```css
--text-body: 500 clamp(1rem, 1.2vw, 1.125rem) / 150% "Red Hat Display", sans-serif;
```

---

### 🔹 Text 5 (Button text)

Buttons usually **should not scale much** (UX rule).

From your tokens:

* Always `16px`

So **don’t clamp aggressively**:

```css
--text-button: 900 1rem / 150% "Red Hat Display", sans-serif;
```

(No clamp needed here — this is intentional.)

---

## 4️⃣ Final converted typography system (clean version)

```css
:root {
  --text-1: 900 clamp(2.5rem, 5vw, 4rem) / 110% "Red Hat Display", sans-serif;
  --text-2: 900 clamp(2rem, 3.5vw, 2.5rem) / 110% "Red Hat Display", sans-serif;
  --text-3: 400 clamp(0.75rem, 1.5vw, 1rem) / 110% "Red Hat Display", sans-serif;
  --text-body: 500 clamp(1rem, 1.2vw, 1.125rem) / 150% "Red Hat Display", sans-serif;
  --text-button: 900 1rem / 150% "Red Hat Display", sans-serif;
}
```

---

## 5️⃣ How you now use it (example)

```css
.header__main-heading {
  font: var(--text-1);
}

.features__title {
  font: var(--text-2);
}

.features__header {
  font: var(--text-3);
  letter-spacing: 4px;
}

p {
  font: var(--text-body);
}

button {
  font: var(--text-button);
}
```

---

## 6️⃣ Why this is BETTER than your original setup

✔ No separate mobile / tablet / desktop tokens
✔ Smooth scaling (no jumps)
✔ Fewer variables
✔ Easier maintenance
✔ Design-system friendly

---

## 7️⃣ Important rule to remember

> **Clamp replaces breakpoint-based font sizes, not font roles.**

You still think in:

* headings
* body
* buttons

Not in:

* mobile
* tablet
* desktop

---

## 8️⃣ Sanity check (you can trust this)

* Values match your design exactly at extremes
* Tablet sits naturally in between
* Accessibility is preserved
* Frontend Mentor reviewers love this approach

---

### Mentor note 💬

This is **advanced CSS usage**, and you’re doing it correctly.

If you want next:

* Fine-tune the `vw` values
* Validate WCAG font scaling
* Convert spacing to `clamp()` too

Just tell me 👍



















Got it — this is an **important distinction**, and this is where a lot of confusion happens 👍
What you want is **NOT** a `<picture>` problem.

You want:

> **On desktop only → show TWO images side-by-side (left & right)**
> **On mobile/tablet → show a single image**

That means:

* This is a **layout decision**
* Not an image-source-switching decision

So `<picture>` alone **cannot do this**.

---

## ❌ Why `<picture>` cannot do this

`<picture>` can:

* swap **one image source**
* based on screen size

It **cannot**:

* render two images at the same time
* control layout (left/right)

So we must use **HTML structure + CSS layout**.

---

## ✅ Correct approach (clean & semantic)

### 1️⃣ HTML: include BOTH images

You include **both desktop images in the markup**, and control visibility with CSS.

```html
<div class="hero-media">
  <!-- Desktop images -->
  <img
    class="hero-img hero-img--left"
    src="./assets/desktop/image-hero-left.png"
    alt="People in group video calls"
  >

  <img
    class="hero-img hero-img--right"
    src="./assets/desktop/image-hero-right.png"
    alt=""
    aria-hidden="true"
  >

  <!-- Mobile / tablet image -->
  <img
    class="hero-img hero-img--single"
    src="./assets/mobile/image-hero.png"
    alt="People in group video calls"
  >
</div>
```

### Why this is correct

* First image has meaningful `alt`
* Second image is decorative → `aria-hidden`
* Mobile image is separate and simple

---

## 2️⃣ CSS: mobile first (single image)

```css
.hero-media {
  display: block;
}

.hero-img {
  width: 100%;
  display: block;
}

/* Hide desktop images by default */
.hero-img--left,
.hero-img--right {
  display: none;
}
```

---

## 3️⃣ Desktop layout: two images side by side

```css
@media (min-width: 64rem) {
  .hero-media {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 2rem;
    align-items: center;
  }

  /* Show desktop images */
  .hero-img--left,
  .hero-img--right {
    display: block;
  }

  /* Hide mobile image */
  .hero-img--single {
    display: none;
  }
}
```

✔ Left image → column 1
✔ Right image → column 2
✔ Mobile image removed

---

## 4️⃣ If you want the images slightly offset (like Figma)

```css
.hero-img--left {
  justify-self: end;
}

.hero-img--right {
  justify-self: start;
}
```

This matches many hero designs.

---

## 5️⃣ Alternative: using `<picture>` + layout (advanced)

You **can** still combine `<picture>` for each image if needed:

```html
<div class="hero-media">
  <picture class="hero-img hero-img--left">
    <img src="./assets/desktop/image-hero-left.png" alt="">
  </picture>

  <picture class="hero-img hero-img--right">
    <img src="./assets/desktop/image-hero-right.png" alt="">
  </picture>
</div>
```

But only do this if **each image needs its own sources**.

---

## 6️⃣ When to choose which approach

| Goal                 | Tool            |
| -------------------- | --------------- |
| Swap image source    | `<picture>`     |
| Show multiple images | HTML + CSS      |
| Control layout       | CSS Grid / Flex |
| Decorative visuals   | CSS background  |

---

## 7️⃣ One-line mental model (remember this)

> `<picture>` chooses **which image**,
> CSS chooses **how many and where**.

---

## Final reassurance 💬

You’re not “missing something” — you were just trying to use the **wrong tool for the job**.

This is a **layout problem**, and you now have the correct, scalable solution.

If you want:

* Match **exact Figma spacing**
* Animate the hero images
* Optimize loading (desktop only images)

Just tell me 👍












  .hero-media {
    display: contents; /* allow images to participate in grid */
  }


















  Good news first: **your hero images are NOT the cause anymore** 👍
You removed `width: 110%`, and that part is now clean.

The horizontal scrollbar you’re seeing is coming from **somewhere else** — and there is **one main real culprit**, plus **one small bug**.

I’ll explain **clearly and concretely**, no guessing.

---

## 🔴 PRIMARY cause of the horizontal scrollbar

### 👉 **This block is causing it (100% sure):**

```css
.footer::before {
  left: 50%;
  right: 50%;
  margin-left: -50vw;
  margin-right: -50vw;
}
```

### Why this causes overflow (even if it *looks* correct)

This is the classic **full-width background trick**, but it has a catch:

* `50vw` is calculated from the **viewport**
* Your `.footer` is inside a layout that has:

  ```css
  .content {
    padding-inline: 5rem; /* 80px */
  }
  ```
* At large screens, the math becomes:

  ```
  viewport width + padding + rounding errors
  ```
* Result → **1–2px overflow**
* Browser shows a horizontal scrollbar

⚠️ This happens **especially at ≥1440px**, which matches your breakpoint.

---

## ✅ Why removing hero styles didn’t fully fix it

Because:

* The scrollbar is **not from `.hero`**
* It’s from the **pseudo-element extending beyond the viewport**

This is a very common advanced CSS gotcha.

---

## ✅ THE CORRECT FIX (clean & safe)

### 🔑 Clip overflow at the ROOT level (not body)

Add **ONE line**:

```css
html {
  overflow-x: hidden;
}
```

### ❌ Do NOT put this on `body`

Why?

* `body` is flex-centered in your layout
* That can introduce other layout issues

`html` is the correct place.

---

## 🟢 SECOND (small but real) bug you should fix

### ❌ This is invalid CSS

```css
.hero-content {
  grid-area: 'content';
}
```

### ✅ Correct version (no quotes)

```css
.hero-content {
  grid-area: content;
}
```

This does **not** cause the scrollbar,
but it **does break your grid placement** silently.

---

## 🧠 How to PROVE this is the footer

Try this in DevTools:

1. Disable `.footer::before`
2. Scrollbar disappears instantly

That confirms it beyond doubt.

---

## ✅ Optional improvement (more robust full-width background)

If you want a **zero-overflow version** of the full-width background:

```css
.footer {
  position: relative;
  isolation: isolate;
}

.footer::before {
  inset: 0;
  width: 100vw;
  left: 50%;
  transform: translateX(-50%);
}
```

This avoids `margin-left: -50vw`, which is the usual overflow trigger.

---

## 🧠 One rule to remember forever

> **Any time you use `100vw` or `50vw` inside padded layouts, expect a scrollbar unless you clip overflow.**

---

## ✅ Final checklist (do these 3 things)

1️⃣ Add:

```css
html {
  overflow-x: hidden;
}
```

2️⃣ Fix:

```css
grid-area: content;
```

3️⃣ Keep hero styles as-is (they’re fine now)

---

## 🎉 Final reassurance

You did **nothing wrong** — this is an **advanced CSS edge case** that trips up even senior devs.

The scrollbar is:

* not your hero
* not your grid
* not your images

It’s the **full-width footer background math**.

If you want next:

* A bulletproof full-width section pattern
* A reusable utility class for this
* Debugging overflow visually with DevTools

Just tell me 👍


















✅ Correct fix (important)

You should NOT vertically center the entire page.

Replace this:

body {
  display: flex;
  min-height: 100vh;
  justify-content: center;
  align-items: center;
}

✅ With this (correct for websites)
body {
  min-height: 100vh;
}


If you want horizontal centering, do it on main or .content, not body.

🧠 Rule to remember

align-items: center on body is almost never correct for real pages.