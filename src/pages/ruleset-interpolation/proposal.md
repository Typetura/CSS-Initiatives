---
layout: /src/layouts/Default.astro
---

<!-- @format -->

# Ruleset interpolation proposal

The web is fluidly responsive and authors require more fluid control over their styles. Currently entire CSS rulesets can be changed at specific breakpoints with container and media queries. However there is no way to interpolate these rulesets. This is particularly problematic when it comes to responsive styling of text. Websites like [The Outline](https://theoutline.com/), [Stripe](https://stripe.com/), [HEX](https://hex.xyz/Margo/) and [Elliot Jay Stocks](https://elliotjaystocks.com/) add numerous breakpoints to control how text is styled with growing and shrinking pages. Interpolation of rulesets across container or viewport sizes would enable better design fidelity across a greater range of viewports with cleaner and more manageable code.

---

### Todo

Next step create a detailed proposal and clear expaliner documents with pros and cons for various approaches.

- Create a more specific proposal for a <abbr title="Cascading Style Sheets Working Group">CSSWG</abbr> and <abbr title="Technical Architecture Group">TAG</abbr>.
  - Outline problems and opportunities
  - Rough CSS proposal
  - Other possible options and solutions
    - Expand on existing solutions like `clamp()` and their shortcomings
  - Assess developer interest
  - A through rundown of the alternatives
- Explainers help get through TAG
  - Indicate where input would be useful
  - Link to the full explainer in a Github issue with proposal

---

**Placeholder for now. Things will likely change here relatively quickly.**

Developing and documenting polyfills for CSS that will have an impact on the typographic quality (and more!) of the web. Ultimately the end result would be to get Miriam Susanne’s Interpolated Values specification into CSS. The same end result can be done with the current specification and a few CSS tricks but now browsers support it. This will enable fluid control over styles across viewport and container widths, not rigid breakpoints but full interpolation between all values.

Success would be this technique replacing clamp for fluid typography on the web. There are numerous issues with clamp for typography from it’s unintuitive syntax to lack of control regarding how things scale at different points. This approach addresses all these issues. Seeing trends of this approach overtaking clamp on web surveys and trending topics would be a success.
