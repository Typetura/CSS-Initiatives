---
layout: /src/layouts/Default.astro
---

<!-- @format -->

# Ruleset interpolation

The web is fluidly responsive and authors require more fluid control over their styles. Currently entire CSS rulesets can be changed at specific breakpoints with container and media queries. However there is no way to interpolate these rulesets. This is particularly problematic when it comes to responsive styling of text. Websites like [The Outline](https://theoutline.com/), [Stripe](https://stripe.com/), [HEX](https://hex.xyz/Margo/) and [Elliot Jay Stocks](https://elliotjaystocks.com/) add numerous breakpoints to control how text is styled with growing and shrinking pages. Interpolation of rulesets across container or viewport sizes would enable better design fidelity across a greater range of viewports with cleaner and more manageable code.

### [Explainer](/ruleset-interpolation/explainer)