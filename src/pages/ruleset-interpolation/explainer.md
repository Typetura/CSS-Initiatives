---
layout: /src/layouts/Default.astro
---

<!-- @format -->

# Ruleset interpolation Explainer

## Authors

- Scott Kellum
- Miriam Suzanne

## Participate

Please leave feedback by filing issues on [this page’s github repository](https://github.com/Typetura/CSS-Initiatives/issues/new?labels=ruleset+interpolation+explainer).

Comment on the [CSSWG issue and discussion](https://github.com/w3c/csswg-drafts/issues/6245#issuecomment-926351855)

Proto implementation: [Typetura](https://github.com/typetura/typetura)

## Table of contents

- [Introduction](#introduction)
- [Functionality goals](#functionality-goals)
- [Proposed solutions](#proposed-solutions)
  - [Container-animation](#container-animation)
  - [Animation attachment to a container](#animation-attachment-to-a-container)
  - [Animation timeline](#animation-timeline)
- [Workaround](#workaround)
- [Key scenarios](#key-scenarios)
  - [Reduce excessive breakpoints](#reduce-excessive-breakpoints)
  - [Consolidate styles](#consolidate-styles)
  - [Changing typesetting around layout shifts](#changing-typesetting-around-layout-shifts)
  - [Smooth layout shifts on resize](#smooth-layout-shifts-on-resize)
  - [Root sizing](#root-sizing)
- [Accessibility](#accessibility)
- [Stakeholder Feedback / Opposition](#stakeholder-feedback--opposition)
- [References & acknowledgements](#references--acknowledgements)

## Introduction

There are times when it is ideal to have a gradation of change between viewport and component sizes as opposed to specific points defined in media queries and container queries. As more device types like watches and foldables enter the web ecosystem as well as more complex layouts being used the need for this level of control increases. This control is especially useful for [typography and spacing within layout](#key-scenarios).

Currently some degree of this can be achieved with `clamp()`, but this doesn’t follow the container sizing mental model used in media/element queries, is limited to a single value, is limited to length typed units, and lacks easing control. [A workaround to gain back all this functionality](#workaround) is possible by passing container width into the delay of an animation function. There are opportunities to make this cleaner and more clear.

## Functionality goals

- The ability to interpolate styles across container and viewport widths.
- Interpolate all interoperable value types including `length`, `color`, and `number`.
- Define the easing of the interpolation.
- Define the start and end size of the interpolation.
- Ideally more than two points can be interpolated. For example an inline-size of 20rem, 40rem, and 80rem can have different rulesets.
- Ideally multiple properties can be interpolated at once. It’s rare that one will need to interpolate one `property: value` pair to change at a time.

## Example

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
    font-size: 3rem;
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

## Proposed solutions

Below are three different proposals for this functionality. I prefer the [container-animation](#container-animation-ideal-solution) direction as I find it to be the most intuitive, but there are advantages to the other two directions as well.

### Container-animation (ideal solution)

This proposal creates a specific property focused on container interpolation as opposed to relying on animation. This sheds the baggage of time-based logic enabling `container-keyframes` to contain length based units. This proposal was [originally discussed in this gist](https://gist.github.com/scottkellum/0c29c4722394c72d311c5045a30398e5).

The biggest _pro_ for this direction is the ability to use length values as the keyframes themselves, so it’s clear what context you are in and what is changing within that context.

The biggest _con_ for this direction is that it’s largely new CSS and a bit of a departure from scroll timeline and `mix()`.

```css
article {
  container-type: inline-size;
}

.headline {
  container-timeline-name: headline;
  container-timeline-axis: size(inline);
  container-timeline-timing-function: ease-in-out;
}

@container-keyframes headline {
  10rem {
    font-size: 1.2rem;
    font-weight: 900;
    color: hsl(330, 96%, 15%);
  }
  40rem {
    font-size: 3rem;
    font-weight: 600;
    color: hsl(330, 96%, 45%);
  }
}
```

#### Advantages

- A clear and intuitive syntax
- Relies on container query syntax and mental model
- Length units can be used in keyframes
  - These units can also be mixed, so things can be interpolated from a size of 100px to a size of 30rem. As one example this may be useful in interpolating an icon’s appearance from its minimum pixel-based size to a size based on the text column it sits within.
- Can assume an `animation-fill-mode` of `both` as default

#### Disadvantages

- A whole new spec to maintain as opposed to expanding the existing animations/scroll timeline spec
- Diffucult to override styles in keyframes

#### Open questions

-  What styles are applied when the `animation` property is used on this element?
  - Ideally the animation would override styles it applies, but not completely nullify styles within the container interpolation.

### Mix timeline

[Fantasai drafted this proposal](https://github.com/w3c/csswg-drafts/issues/6245#issuecomment-926351855) with Miraim Suzanne that achieves most of the [functionality goals](#functionality-goals) outlined above. It expands upon the [mix proposal for interpolating values](https://github.com/w3c/csswg-drafts/issues/581#issuecomment-926353789).

The biggest _pro_ for this direction is that styles can be easily overwritten in the cascade. This is an annoyance with styles in keyframes.

The biggest _con_ for this direction is that it doesn’t meet the last two [functionality goals](#functionality-goals). These are ideal and not must-haves, but still a concession on functionality.

```css
article {
  container-type: inline-size;
}

@timeline headline {
  type: container;
  feature: width;
  from: 10rem;
  to: 40rem;
}

.headline {
  font-size: mix(headline ease-in-out; 1.2rem; 3rem);
  font-weight: mix(headline ease-in-out; 900; 600);
  color: mix(headline ease-in-out; hsl(330, 96%, 15%); hsl(330, 96%, 45%));
}
```

#### Advantages

- A Simple syntax
- Easy to override styles
- Relies on container query syntax and mental model
- No need to worry about `animation-fill-mode` at all

#### Disadvantages

- Lacks support for the following [functionality goals](#functionality-goals)
  - Ideally more than two points can be interpolated. For example an inline-size of 20rem, 40rem, and 80rem can have different rulesets
  - Ideally multiple properties can be interpolated at once. It’s rare that I need only one `property: value` pair to change at a time
- While it’s very simple, it’s not as clear or intuitive

### Animation timeline

This direction is more aligned with scroll-timeline. It satisfies all of the [functionality goals](#functionality-goals).

The biggest _pro_ for this direction is that it is so close to the existing scroll timeline spec. This direction feels clear and achievable.

The biggest _con_ is that it’s more verbose than the other directions without adding as much clarity.

```css
article {
  view-timeline-name: article;
  view-timeline-axis: size(inline);
}

.headline {
  animation: headline both ease-in-out;
  animation-timeline: article;
  animation-delay-start: inline-size 10rem;
  animation-delay-end: inline-size 40rem;
}

/* Standard animation keyframes */
@keyframes headline {
  from {
    font-size: 1.2rem;
    font-weight: 900;
    color: hsl(330, 96%, 15%);
  }
  to {
    font-size: 3rem;
    font-weight: 600;
    color: hsl(330, 96%, 45%);
  }
}
```

#### Advantages

- Relies on scroll-timeline syntax and mental model
- Relies on animation and keyframe syntax and mental model
- Aligned with animation spec for future changes and maintenance

#### Disadvantages

- Doesn’t allow for length based units in keyframes
- `fill-mode` default of `none` is not ideal
- Diffucult to override styles in keyframes

#### Open questions

Some details could be worked out around how `animation-timeline` and `view-timeline` work.

## Workaround

A workaround is possible now according to the latest CSS spec and is waiting for browsers to implement unit division _(the `/ 1px` portions of animation delay)_ to function properly:

```css
article {
  container-type: inline-size;
}
.headline {
    animation-name: headline;
    animation-timing-function: ease-in-out;
    --min: 10rem;
    --max: 40rem;

    animation-delay: calc(-1s * (100cqi - var(--min, 0) / 1px) / ((var(--max, 0) - var(--min, 0)) / 1px));
    animation-duration: 1s;
    animation-iteration-count: 1;
    animation-fill-mode: both;
    animation-play-state: paused;
}
@keyframes headline {
  from {
    font-size: 1.2rem;
    font-weight: 900;
    color: hsl(330, 96%, 15%);
  }
  to {
    font-size: 3rem;
    font-weight: 600;
    color: hsl(330, 96%, 45%);
  }
}
```

This is the same code Typetura uses, except Typetura uses JavaScript to attach `resizeObserver`s to container elements, passing a `var(--width)` value into the animation delay function for better browser support.

## Key scenarios

### Reduce excessive breakpoints

Currently the following websites use numerous breakpoints to define how typesetting changes at different screen sizes:
[The Outline](https://theoutline.com/), [HEX](https://hex.xyz/Margo/) and [Elliot Jay Stocks](https://elliotjaystocks.com/).

### Consolidate styles

[The Atlantic](https://www.theatlantic.com/) has a typographically rich home page on desktop with headlines of various sizes based on their container size. All of these headline styles could be consolidated into one interpolated ruleset for this layout with the additional bonus of working across viewport sizes. Here is a demo of [The Atlantic with an interpolated headline style](https://demos.typetura.com/the-atlantic).

### Changing typesetting around layout shifts

The [Stripe](https://stripe.com/) home page changes its type styles for a layout shift. If these styles were interpolated and bound to the text container itself, then as the layout shift happens the text would automatically resize appropriately. Here is an example of [what this type of layout might look like with interpolated styles](https://demos.typetura.com/magazine.html).

### Smooth layout shifts on resize

Lynn Fisher used Typetura to interpolate rulesets with CSS for [her latest 2021 website redesign](https://lynnandtonic.com/). This would all be possible natively in CSS with ruleset interpolation. Additionally more subtle applications of this can be applied, like the way the floating images slide in and out of the text column where there is space on [this “Explore” demo](https://demos.typetura.com/magazine.html).

### Root sizing

As devices get increasingly small, controlling page scaling with the root font size is more useful. Because most websites don’t work well on watches, Apple adapts them for smaller screens unless `<meta name="disable-adaptations" content="watch">` is in the `<head>`. The ideal solution here is interpolating the root font size with an `ease-out` timing function. You can see how [the “Explore” demo](https://demos.typetura.com/magazine.html) scales to extremely small sizes.

## Accessibility

Any time you change font size you run the risk of falling out of compliance with WCAG SC 1.4.4 and this still holds true with this feature. The success criteria may change in the future based on this open [issue with the WCAG on 1.4.4](https://github.com/w3c/wcag/issues/1671). There is currently no way to identify and compensate for page scaling accessibility issues on desktop browsers (cmd +) and there is an open issue on how we might surface and solve for [page scaling issues here](https://github.com/w3c/csswg-drafts/issues/6869).

## Stakeholder Feedback / Opposition

- Chromium : Waiting on feedback
- Gecko : Waiting on feedback
- Webkit : Waiting on feedback

## References & acknowledgements

- [Scott Kellum](https://css.typetura.com)
- [Miriam Suzanne](https://css.oddbird.net/) for proposal collaboration
  - [interpolation notes from Miriam](https://css.oddbird.net/rwd/interpolation/)
- [Fantasai](https://fantasai.inkedblade.net/)
  - [Interpolation proposal comment](https://github.com/w3c/csswg-drafts/issues/6245#issuecomment-926351855)
- [Nicole Sullivan](https://twitter.com/stubbornella) for general advice and proposal feedback
- [Google UI Fund](https://web.dev/ui-fund/) for project support
- [Ana Monroe](https://anamonroe.com/) for editing and design feedback

#### Additional thanks to support, feedback, and inspiration from

- [Tim Brown](https://tbrown.org/)
- [Florian Schulz](https://florianschulz.info/)
- [Jen Simmons](https://jensimmons.com/)

## Changelog

### 2022.11.3

- Rewrite of proposals after feedback from Miriam and more research done on how the scroll timeline spec has changed.

### 2022.10.31

- First draft