---
layout: /src/layouts/Default.astro
---

<!-- @format -->

# Ruleset interpolation roadmap

Typetura’s underlying technology is a working and production ready prototype of this functionality. It enables interpolating rulesets as containers resize. Currently this is done with the CSS animation API and JavaScript to identify container size. The first step in making this work with only CSS would be to support unit division in `calc()`.

✅ Container queries, and specifically defining containers and `cqi` units.

⛔️ Calc unit interpolation, where when one value that is divided by another value of a similar type, the result is a unitless representation of the denominator. So `1in / 1px` would equal `96`.

```css
html,
main,
article,
section,
.typetura {
  container: inline-size;
}

* {
  animation: 1s var(--timeline-ease, linear) calc(
      (-1s) * (100cqi / 1px) / var(--timeline-max, 1000)
    )
    1 both paused var(--timeline, none);
}

h1 {
  --timeline: h1;
  --timeline-min: 100; /* Not included in the function yet */
  --timeline-max: 1000;
  --timeline-ease: ease-in;
}
@keyframes h1 {
  0% {
    /* timeline-min styles */
    font-size: 1.5rem;
    line-height: 1.2;
  }
  100% {
    /* timeline-max styles */
    font-size: 6rem;
    line-height: 1;
  }
}
```

## How is handled in Typetura currently?

Typetura identifies possible containers as selectors in a JS array, uses mutation observers to watch as the DOM is populated, adds resize observers to those containers, and writes the width (unitless px value) to a CSS variable to be fed into the Typetura function.

This can be shortened into two things: identifying containers and parsing the width.

- Identifying containers can be replaced by the new `container: inline-size` property from the [container query spec](https://drafts.csswg.org/css-contain-3/#container-lengths).
- Parsing width can be replaced with `(100cqi / 1px)`. This requires the [new unit spec](https://drafts.csswg.org/css-values-4/#changes-recent).

## Ruleset interpolation

The above is a stop-gap to bring ruleset interpolation to CSS without the need for JS. However this is a relatively complex use of animation and authorship would be enhanced with a [proper ruleset interpolation spec](proposal).

## Polyfills

To assist adoption both `calc()` division by units and ruleset interpolation will need to be polyfilled. As browser support gets better this will reduce the reliance on JS to make expensive `resizeObserver` calls.
