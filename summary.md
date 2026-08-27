---
title: "Social Links Profile — Session Learnings"
date: 2026-08-27
tags:
  - frontend-mentor
  - html
  - css
  - accessibility
  - web-design
challenge: "Social links profile"
level: Newbie
related:
  - "[[challenge/README.md]]"
  - "[[code/index.html]]"
  - "[[code/css/style.css]]"
---

# Social Links Profile — What I Learned

A summary of a review-and-refine session on the **Frontend Mentor "Social links profile"** challenge. The theme of the whole session: **reason about *why*, not just *what*** — and let the "why" drive the code.

> [!tip] TL;DR
> - Choose HTML elements by what content **means**, not how it **looks**.
> - Put **decoration** in CSS, **content** in HTML.
> - `border-radius: 50%` only makes a circle on a **square** box.
> - `width` = a **target** ("be this"); `max-width` = a **ceiling** ("at most this").
> - Inside a flex container, items **shrink by default** (`flex-shrink: 1`).
> - Build components that **don't assume anything about their parent**.
> - Accessibility = *"will this work for someone who browses differently than me?"*

---

## 1. Semantic HTML: meaning, not looks

**The big idea:** pick an element for what the content *is*, not for how it happens to be styled.

- The bio *looked* like a quote (it had quotation marks), but it isn't a quotation **from another source** — it's just Jessica's own tagline. So the right element is `<p>`, **not** `<blockquote>`.
- `<blockquote>` has a specific meaning: *content quoted from somewhere else*. Using it for a normal tagline would "lie" to screen readers and search engines.

**Rule of thumb:** if you're reaching for an element because of how it looks, stop and ask *what does this content actually mean?*

---

## 2. Content vs. presentation (HTML vs. CSS)

The quotation marks around the bio are **decoration**, not **content** — so they belong in CSS, not in the HTML text.

```css
.author-quote::before { content: "“"; }  /* opening */
.author-quote::after  { content: "”"; }  /* closing */
```

- Pseudo-elements (`::before` / `::after`) let you add decorative text via CSS.
- **Opening vs. closing quotes are different characters:** `“` (left / opening) and `”` (right / closing). Using the same one for both makes the trailing quote curl the wrong way.

---

## 3. Typography: be explicit, don't lean on defaults

- An `<h1>` *looks* bold only because browsers bold it **by default**. Relying on invisible defaults can surprise you later — state the `font-weight` you actually intend.
- The location text needed `font-weight: 600` to match the design; without it, it fell back to the normal `400`.

---

## 4. The circle-that-was-an-ellipse (aspect ratio)

**Symptom:** the avatar rendered as an ellipse even though `border-radius: 50%` was set.

**Key fact:**
> `border-radius: 50%` produces a **circle** only when the box is a **perfect square**. On a rectangle it makes an **ellipse**.

**Why the box was a rectangle:** `width` was set in CSS, but the **height came from the HTML `height="176"` attribute**, so the two dimensions disagreed and the image was distorted.

**The fix:**
```css
.author-img img {
  width: 100%;
  height: auto;   /* height follows width → proportions preserved → square → circle */
}
```

**Deeper lesson — two kinds of boxes:**
- An `<img>` is a **replaced element**: it has an *intrinsic natural size* (here 176×176) baked into the file. That's why `height: auto` was **load-bearing** on the image.
- A `<div>` is a plain **block box**: it has **no intrinsic size** (width fills its parent, height fits its content). So `height: auto` on the container was just **restating the default** — redundant.

**Skill unlocked:** use browser **DevTools → Inspect** to read an element's *actual* rendered width/height and see the box model.

---

## 5. `width` vs `max-width` (the recurring hero of this session)

| Property | Meaning | Analogy |
|---|---|---|
| `width` | a **target** — "*be* exactly this wide" | a fixed command |
| `max-width` | a **ceiling** — "at most this wide, but you can be smaller" | an upper limit |

Things this distinction explained:

- **Why the card wouldn't grow:** raising `max-width` from `39.5rem` to `50rem` did nothing, because a ceiling doesn't *push* an element wider — and with no `width` set, the card only sized to its content. To make it wider you need a `width` (a target).
- **The image-width footgun:** a *global* `img { width: 100% }` would stretch a tiny 32px icon up to full width. Safer default:
  ```css
  img { max-width: 100%; height: auto; }
  ```
  `max-width: 100%` only *shrinks* oversized images; it never *forces* small ones to grow.
- **Scoping beats global:** targeting `.author-img img` instead of all `img` means `width: 100%` is safe — it only affects the one image meant to fill its box.

**`max-width` as a defensive habit (balanced against YAGNI):**
- Preferring `max-width` for content is a good habit *because it costs nothing* — no extra complexity, just a more robust default.
- The discipline that makes it *good* rather than risky: since `max-width` lets a box shrink, **verify the shrunk state still looks right.**
- Sometimes rigidity is correct (an icon that must be exactly 24px) — the skill is knowing which behavior you want.

---

## 6. Flexbox sizing & responsiveness

**Puzzle:** I set a fixed `width: 50rem` and expected it to stay rigid (and cause horizontal scroll on small screens). Instead it **shrank**. Why?

**Answer:** the card is a **flex item** (its parent is `display: flex`). Every flex item has an invisible default:
```css
flex-shrink: 1;   /* "you may shrink below your set size if space runs out" */
```
So on a flex item, `width` behaves like a **preferred / starting size** that can be compressed — which is *why* it shrank. That shrinking is flexbox **protecting you from overflow**, not a bug.

- `flex-shrink: 0` would make it truly rigid — but then it would **overflow** a 320px phone. Usually **not** what you want.
- My earlier intuition ("fixed width → overflow + scroll") was **correct for normal block flow** — flexbox just plays by different rules.

---

## 7. The robust, portable sizing pattern

```css
.author-profile-card {
  width: 39.5rem;   /* target: be this wide when there's room */
  max-width: 100%;  /* ceiling: never wider than my container */
}
```

- `width` sets the desired size **even when the content doesn't need it**.
- `max-width: 100%` guarantees it **never overflows**, in *any* container.
- Note: in *this* flex layout, `flex-shrink` already handles shrinking, so `max-width: 100%` is partly redundant here — **but** it makes the component robust anywhere.

---

## 8. Component thinking

> A well-built component **owns its own constraints** and makes **no assumptions about its parent**.

- `max-width: 100%` means the card promises *"wherever you drop me — flex, grid, or a plain div — I won't overflow."* That's what makes it reusable/exportable.
- **Deliberate redundancy vs. cargo-culting:** a small, consistent redundancy that buys robustness is fine **when you can justify it**. The danger is only ever copying a pattern you *can't* explain.

---

## 9. Accessibility — *"will this work for someone browsing differently?"*

**Alt text — "what would a screen reader say?"**
- A screen reader already announces *"image"*, so `alt="Profile picture of…"` is redundant → just describe the essence: `alt="Jessica Randall"`.
- **Decorative images:** use an **empty** alt `alt=""` to tell the screen reader *"skip me."* Chosen here because the avatar sits right next to an `<h1>` with the same name (avoiding a double announcement).
- ⚠️ **Empty `alt=""` ≠ missing alt.** Omitting `alt` entirely often makes screen readers read the **filename** out loud. Decorative = `alt=""`, never "no alt."
- Guiding idea: **signal vs. noise** — removing redundant announcements is a kindness.

**Focus states — "how does a keyboard user know where they are?"**
- Some people navigate with the **Tab key** only. A visible focus style is their "you are here" marker.
- The browser's **default focus outline** is a built-in accessibility feature. ⚠️ **Never** remove it with `outline: none` unless you replace it with something equally visible.
- `:focus-visible` is a nicer, keyboard-aware way to style the ring (a rabbit hole for later).

**Contrast (habit to build):** drop color pairs into a **contrast checker** (e.g. WebAIM) and ask *"could someone with low vision read this?"*

---

## 10. Margin shorthand (quick reference)

```css
margin: 1rem;      /* all four sides */
margin: 1rem 0;    /* vertical | horizontal  → top&bottom = 1rem, left&right = 0 */
```
- **Two values:** `vertical horizontal`.
- **Four values:** clockwise from top → `top right bottom left`.

---

## Meta-skills I practised

- **Predict before you test.** Guessing the outcome *first* (then checking in the browser) builds real intuition.
- **Use DevTools to see reality**, not just assume what the CSS "should" do.
- **Ask "why", not just "what".** Every fix this session came from understanding the underlying rule — which means it transfers to the next project.

---

## Checklist to reuse on future challenges

- [ ] Element chosen by **meaning**, not appearance?
- [ ] Decoration in CSS, content in HTML?
- [ ] Images: `max-width: 100%; height: auto;` so they scale and keep proportions?
- [ ] Intentional about `width` (target) vs `max-width` (ceiling)?
- [ ] Does the layout survive **320px → large screens** without horizontal scroll?
- [ ] Meaningful `alt` text (or deliberate `alt=""` for decorative)?
- [ ] Visible focus state for keyboard users (outline **not** removed)?
- [ ] Colors pass a **contrast** check?
