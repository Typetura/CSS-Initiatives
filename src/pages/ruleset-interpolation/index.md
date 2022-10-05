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

## Desired outcome

<div class="typetura demo" id="demo">
    <h1 class="headline">Resize me</h1>
</div>
<style>
.demo {
    position: relative;
    display: inline-block;
    width: 25rem;
    max-width: 100%;
    height: 10rem;
    border-radius: 0.25rem;
    background-color: #eee;
    resize: horizontal;
    overflow: hidden;
}
.headline {
    --min: 200;
    --max: 800;
    margin-block: 2rem;
    line-height: 1;
    animation: 1s ease-in-out calc(-1s * (var(--width, 0) - var(--min)) / (var(--max) - var(--min))) 1 both paused headline;
}
@keyframes headline {
  from {
    font-size: 1.2rem;
    font-weight: 900;
    color: hsl(330, 96%, 15%);
  }
  to {
    transform: 3rem;
    font-weight: 600;
    color: hsl(330, 96%, 45%);
  }
}
</style>
<script>
    let demo = document.getElementById('demo');
    const resizeObserver = new ResizeObserver((entries) => {
    for (const entry of entries) {
        if (entry.contentBoxSize) {
        // Firefox implements `contentBoxSize` as a single content rect, rather than an array
        const contentBoxSize = Array.isArray(entry.contentBoxSize) ? entry.contentBoxSize[0] : entry.contentBoxSize;
        entry.target.style.setProperty('--width', contentBoxSize.inlineSize);
        }
    }
    });
    resizeObserver.observe(demo);
</script>

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
