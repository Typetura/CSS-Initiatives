---
layout: /src/layouts/Default.astro
---

<!-- @format -->

# Ruleset interpolation

The web is fluidly responsive and authors require more fluid control over their styles. Currently entire CSS rulesets can be changed at specific breakpoints with container and media queries. However there is no way to interpolate these rulesets. This is particularly problematic when it comes to responsive styling of text. Websites like [The Outline](https://theoutline.com/), [Stripe](https://stripe.com/), [HEX](https://hex.xyz/Margo/) and [Elliot Jay Stocks](https://elliotjaystocks.com/) add numerous breakpoints to control how text is styled with growing and shrinking pages. Interpolation of rulesets across container or viewport sizes would enable better design fidelity across a greater range of viewports with cleaner and more manageable code.

### [Problems and opportunities](/ruleset-interpolation/problems-and-opportunities)

### [Proposal](/ruleset-interpolation/proposal)

### [Roadmap](r/ruleset-interpolation/oadmap)

### [Notes and references](/ruleset-interpolation/notes)

### [Examples](/ruleset-interpolation/examples)



## Next steps

- [ ] Create a more specific proposal for a <abbr title="Cascading Style Sheets Working Group">CSSWG</abbr> and <abbr title="Technical Architecture Group">TAG</abbr>.
  - [ ] Outline problems and opportunities
  - [x] Rough CSS proposal
  - [ ] Other possible options and solutions
    - [ ] Expand on existing solutions like `clamp()` and their shortcomings
  - [ ] Assess developer interest
  - [ ] A through rundown of the alternatives
- [ ] Create explainers to help get through TAG
  - [ ] Indicate where input would be useful
  - [ ] Link to the full explainer in a Github issue with proposal
