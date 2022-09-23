---
layout: /src/layouts/Default.astro
---

<!-- @format -->

# Ruleset interpolation proposal

---

### Todo

- [ ] Create a more specific proposal for a CSSWG spec.
- [ ] Expand on existing solutions like `clamp()` and their shortcomings.

---

**Placeholder for now. Things will likely change here relatively quickly.**

Developing and documenting polyfills for CSS that will have an impact on the typographic quality (and more!) of the web. Ultimately the end result would be to get Miriam Susanne’s Interpolated Values specification into CSS. The same end result can be done with the current specification and a few CSS tricks but now browsers support it. This will enable fluid control over styles across viewport and container widths, not rigid breakpoints but full interpolation between all values.

Success would be this technique replacing clamp for fluid typography on the web. There are numerous issues with clamp for typography from it’s unintuitive syntax to lack of control regarding how things scale at different points. This approach addresses all these issues. Seeing trends of this approach overtaking clamp on web surveys and trending topics would be a success.
