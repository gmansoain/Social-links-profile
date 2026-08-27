# Frontend Mentor - Social links profile solution

This is a solution to the [Social links profile challenge on Frontend Mentor](https://www.frontendmentor.io/challenges/social-links-profile-UG32l9m6dQ). Frontend Mentor challenges help you improve your coding skills by building realistic projects.

## Table of contents

- [About this project](#about-this-project)
  - [How to view this solution](#how-to-view-this-solution)
- [Overview](#overview)
  - [The challenge](#the-challenge)
  - [Screenshot](#screenshot)
  - [Links](#links)
- [My process](#my-process)
  - [Built with](#built-with)
  - [What I learned](#what-i-learned)
  - [Continued development](#continued-development)
  - [Useful resources](#useful-resources)
  - [AI Collaboration](#ai-collaboration)
- [Author](#author)
- [Acknowledgments](#acknowledgments)

## About this project

**Social links profile** is a beginner-level [Frontend Mentor](https://www.frontendmentor.io) challenge. The task is to build a single, self-contained **profile card** — avatar, name, location, a short bio, and a vertical list of social links — matching a provided design as closely as possible, and giving every interactive element clear **hover and focus states**. It's a compact exercise in semantic HTML, responsive layout, and accessible UI.

This repository is my solution, built with plain **HTML and CSS** — no frameworks and no build step.

### How to view this solution

- **Live demo:** [gon-social-links-profile.netlify.app](https://gon-social-links-profile.netlify.app/)
- **Run it locally:** clone this repo and open `code/index.html` in any web browser — there are no dependencies to install.

## Overview

### The challenge

Users should be able to:

- See hover and focus states for all interactive elements on the page

### Screenshot

![Screenshot of the Social links profile solution](./screenshot.png)

### Links

- Solution URL: https://github.com/gmansoain/Social-links-profile.git
- Live Site URL: https://gon-social-links-profile.netlify.app/

## My process

### Built with

- Semantic HTML5 markup
- CSS custom properties (variables)
- Flexbox
- Self-hosted fonts via `@font-face` (Inter), with `font-display: swap`
- `rem` units (using the `62.5%` root font-size technique)
- Responsive layout **without media queries** — a fluid `width` + `max-width: 100%` and `100dvh`
- Accessibility-conscious markup (meaningful/decorative `alt`, visible focus states)
- Supports the full range from **320px to large screens**

### What I learned

This project turned out to be a great lesson in **why** things work, not just **what** to type. A few highlights I want to remember:

**1. Choose HTML by meaning, and keep decoration in CSS.** The bio *looks* like a quote, but it isn't a quotation from another source — so it's a plain `<p>`, and the quotation marks are added as decoration via CSS pseudo-elements (with the correct opening/closing characters):

```html
<p class="author-quote">Front-end developer and avid reader</p>
```
```css
.author-quote::before { content: "“"; }
.author-quote::after  { content: "”"; }
```

**2. `border-radius: 50%` only makes a circle on a *square* box.** My avatar was rendering as an ellipse because its width was set in CSS but its height came from the HTML `height` attribute. Adding `height: auto` lets the height follow the width proportionally, keeping the box square:

```css
.author-img img {
  border-radius: 50%;
  width: 100%;
  height: auto; /* keeps proportions → square box → real circle */
}
```

**3. `width` is a target; `max-width` is a ceiling — and together they make a portable component.** `width` gives the card its size even when the content doesn't need it; `max-width: 100%` guarantees it never overflows, no matter what container it lives in:

```css
.author-profile-card {
  width: 39.5rem;   /* be this wide when there's room */
  max-width: 100%;  /* but never wider than my container, anywhere */
}
```

I also learned that inside a flex container, items shrink by default (`flex-shrink: 1`), which is why a "fixed" width still adapts responsively.

**4. Accessibility is about real people.** The avatar sits right next to a heading with the same name, so I made it decorative with an intentional empty `alt=""` (which tells screen readers to skip it — very different from omitting `alt`). And I kept clear hover **and** focus states so keyboard users always know where they are:

```css
.social-link-item a:hover,
.social-link-item a:focus {
  background-color: var(--main-color);
  color: var(--dark-color-1);
}
```

### Continued development

Areas I want to keep sharpening in future projects:

- Using `:focus-visible` to style focus rings specifically for keyboard users.
- Auditing color contrast with a checker as a standard step.
- Practising a mobile-first workflow with media queries (this build stayed responsive without them, but breakpoints are worth mastering).
- Getting more comfortable with CSS Grid for layouts beyond a single centered card.

### Useful resources

- [MDN: Replaced elements](https://developer.mozilla.org/en-US/docs/Web/CSS/Replaced_element) - Helped me understand why `<img>` (with an intrinsic size) behaves differently from a `<div>`.
- [MDN: `aspect-ratio` & sizing images](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_images) - Cleared up the ellipse-vs-circle issue.
- [CSS-Tricks: A Complete Guide to Flexbox](https://css-tricks.com/snippets/css/a-guide-to-flexbox/) - Explained `flex-shrink` and why flex items adapt.
- [WebAIM Contrast Checker](https://webaim.org/resources/contrastchecker/) - For verifying text is readable.
- [MDN: alt text & decorative images](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/img#alt) - The difference between `alt=""` and no `alt`.

### AI Collaboration

I used **Claude** (via an Obsidian-integrated assistant) as a **learning partner**, not a code generator.

- **How I used it:** It reviewed my first version and gave feedback as a mentor — asking guiding questions and offering hints rather than handing me solutions. I wrote all the code myself and debugged issues (like the ellipse-shaped avatar) by reasoning through its prompts.
- **What worked well:** Being nudged to *predict outcomes before testing* and to use browser DevTools built real intuition. Discussions on `width` vs `max-width`, flexbox shrinking, and accessibility stuck because I had to reason them out rather than copy an answer.
- **What I'd watch for:** It intentionally withheld direct answers, so progress took more effort — which was the point, but worth knowing if you're in a hurry.

## Author
- Frontend Mentor - [@gmansoain](https://www.frontendmentor.io/profile/gmansoain)

## Acknowledgments

Thanks to Frontend Mentor for the challenge and starter files, and to the AI mentor that guided this session by asking the right questions instead of giving away the answers.
