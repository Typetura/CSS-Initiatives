---
layout: /src/layouts/Default.astro
---

<!-- @format -->

As CSS is improving we want to move Typetura’s platform to be CSS only with a polyfill for backwards compatibility. This will make the platform even more performant and approachable while increasing adoption of new CSS.

# Typetura v3

This is an identical CSS API to Typetura v2, except it re-defines how you define Typetura contexts. The benefit here is that JavaScript will no longer be necessary in a browser that supports the latest CSS specs. As this is bleeding edge CSS, no browser supports this CSS yet, not even public nightly builds. Two features need to be polyfilled to make this work:

1. Container queries, and specifically defining containers and `cqi` units.
2. Calc unit interpolation, where when one value that is divided by another value of a similar type, the result is a unitless representation of the denominator. So `1in / 1px` would equal `96`.

```css
/* Identify contexts that have the property:value of container:inline-size */
html,
main,
article,
section,
.typetura {
  container: inline-size;
}

/* Typetura function, low specificity styles that are inactive by default and can be activated with --tt-key */
/* 
   Missing CSS support that needs to be polyfilled are
   1) cqi units and
   2) calc unit interpolation to convert (100cqi / 1px)
      into a unitless value that defines the context
*/
* {
  animation: 1s var(--tt-ease, linear) calc(
      -1s * (100cqi / 1px) / var(--tt-max, 1000)
    )
    1 both paused var(--tt-key, none);
}
```

**How is this different from Typetura v2?**

v2 identifies possible containers as selectors in a JS array, uses mutation observers to watch as the DOM is populated, adds resize observers to those containers, and writes the width (unitless px value) to a CSS variable to be fed into the Typetura function.

This can be shortened into two things: identifying containers and parsing the width.

- Identifying containers can be replaced by the new `container: inline-size` property from the [container query spec](https://drafts.csswg.org/css-contain-3/#container-lengths).
- Parsing width can be replaced with `(100cqi / 1px)`. This requires the [new unit spec](https://drafts.csswg.org/css-values-4/#changes-recent).

# Typetura v4 / Interpolated Values

This marks the end of the line for Typetura’s underlying tech platform as all the features and functionality here will be part of the [Interpolated Values spec](https://css.oddbird.net/rwd/interpolation/). What we need is a polyfill for this spec when it is stable to help people transition on to the new standard with as much compatibility as possible. The CSS API will be similar, but the heavy lifting done by the Typetura function will be obsolete.
