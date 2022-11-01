---
layout: /src/layouts/Default.astro
---

<!-- @format -->

# Ruleset interpolation Explainer

## Authors

- Scott Kellum
- Miriam Suzanne

## Participate

Please leave feedback by filing issues on [this page’s github repositiory](https://github.com/Typetura/CSS-Initiatives/issues/new?labels=ruleset+interpolation+explainer).

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
  - [Consolodate styles](#consolodate-styles)
  - [Changing typesetting around layout shifts](#changing-typesetting-around-layout-shifts)
  - [Smooth layout shifts on resize](#smooth-layout-shifts-on-resize)
- [Stakeholder Feedback / Opposition](#stakeholder-feedback--opposition)
- [References & acknowledgements](#references--acknowledgements)

## Introduction

There are times when it is ideal to have a gradation of change between viewport and component sizes as opposed to specific points as defined in media queries and container queries. As more device types like watches and foldables enter the web ecosystem as well as more complex layouts being used the need for this level of control increases. This control is espeicially useful for typography and spacing within layout.

Currently some degree of this can be achieved with `clamp()`, but this doesn’t follow the container sizing mental model used in media/element queries, is limited to a single value, is limited to length typed units, and lacks easing control. [A workaround to gain back all this functionality](#workaround) is possible by passing container width into the delay of an animation function. There are opportunities to make this cleaner and more clear.

## Functionality goals

- The ability to interpolate styles across container and viewport widths.
- Interpolate all interpolable value types including `length`, `color`, and `number`.
- Define the easing of the interpolation.
- Define the start and end size of the interpolation.
- Ideally more than two points can be interpolated. For example an inline-size of 20rem, 40rem, and 80rem can have different rulesets.
- Ideally multiple properties can be interpolated at once. It’s rare that I need only one `property: value` pair to change at a time.

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

## Proposed solutions

A workaround is possible now within the latest CSS spec and is waiting for browsers to implement unit division _(the `100cqi / 1px` portion of animation delay)_ to function propertly:

```css
article {
  container-type: inline-size;
}
.headline {
    --min: 200;
    --max: 800;
    --name: headline;
    --timing-function: ease-in-out;
    animation:
      1s
      var(--timing-function, linear)
      calc(-1s * (100cqi / 1px - var(--min, 0)) / (var(--max, 0) - var(--min, 0)))
      1
      both
      paused
      var(--name, none);
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
```

### Container-animation

This proposal creates a specific property focused on container interpolation as opposed to relying on animation. This sheds the baggage of time-based logic enabling `container-keyframes` to contain length based units. This proposal was [originally discussed in this gist](https://gist.github.com/scottkellum/0c29c4722394c72d311c5045a30398e5).

```css
article {
  container-type: inline-size;
  container-name: article;
}

.headline {
  container-interpolation-name: headline;
  container-interpolation-timing-function: ease-in-out;
}

@container-keyframes headline (article: inline-size) {
  10rem {
    font-size: 1.2rem;
    font-weight: 900;
    color: hsl(330, 96%, 15%);
  }
  40rem {
    transform: 3rem;
    font-weight: 600;
    color: hsl(330, 96%, 45%);
  }
}
```

#### Advantages

- A simple syntax
- Relies on container query syntax and mental model
- Length units can be used in keyframes
  - These units can also be mixed, so things can be interpolated from a size of 100px to a size of 30rem. As one example this may be useful in interpolating an icon’s appearance from its minimum pixel-based size to a size based on the text column it sits within.

- Does not require duration in order to work
- Can assume an `animation-fill-mode` of `both` as default

#### Disadvantages

- A whole new spec to maintain as opposed to expanding the existing animation spec.

### Animation attachment to a container

This is an extension of `animation` to allow for binding to container queries. A new function here defines the what dimension is being queried as well as the start and end points of the interpolation.

```css
article {
  container-type: inline-size;
  container-name: article;
}

.headline {
  /* Set the animation timeline to start at a conatainer inline-size of 20rem and end at 80rem */
  /* attahment function ( container name and dimention, start size, end size ) */
  animation-attachment: container(article inline-size, 10rem, 40rem);

  /* Define the animation parameters */
  animation-name: headline;
  animation-timing-function: ease-in-out;
  animation-duration: 1s;

  /* If fill mode is not set to `both` then the animation will apply default settings outside the boundaries set in animation-attachment */
  animation-fill-mode: both;
}

/* Standard animation keyframes */
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
```

#### Advantages

- Relies on container query syntax and mental model
- Relies on animation and keyframe syntax and mental model
- Aligned with animation spec for future changes and maintenance

#### Disadvantages

- Doesn’t allow for length based units in keyframes
- Has time-based baggage from animations and duration needs to be defined
- More verbose than [container-animation](#container-animation)
- `fill-mode` default of `none` is not ideal

### Animation timeline

This direction is more aligned with scroll-timeline. It is designed to mirror that spec, changing the syntax to `@query-timeline` as opposed to `@scroll-timeline` to define the query interpolation behavior.

```css
/* Set the animation timeline to start at a conatainer inline-size of 20rem and end at 80rem */
@query-timeline article {
  source: selector("article");
  orientation: "inline";
  length-offsets: 10rem 40rem;
}

.headline {
  /* Set the animation timeline, much like scroll timeline is set */
  animation-timeline: article;

  /* Define the animation parameters */
  animation-name: headline;
  animation-timing-function: ease-in-out;
  animation-duration: 1s;

  /* If fill mode is not set to `both` then the animation will apply default settings outside the boundaries set in animation-attachment */
  animation-fill-mode: both;
}

/* Standard animation keyframes */
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
```

#### Advantages

- Relies on scroll-timeline syntax and mental model
- Relies on animation and keyframe syntax and mental model
- Aligned with animation spec for future changes and maintenance

#### Disadvantages

- Doesn’t allow for length based units in keyframes
- Has time-based baggage from animations and duration needs to be defined
- More verbose than [container-animation](#container-animation)
- `fill-mode` default of `none` is not ideal

## Workaround

A workaround is possible now according to the latest CSS spec and is waiting for browsers to implement unit division _(the `100cqi / 1px` portion of animation delay)_ to function propertly:

```css
article {
  container-type: inline-size;
}
.headline {
    --min: 200;
    --max: 800;
    --name: headline;
    --timing-function: ease-in-out;
    animation:
      1s
      var(--timing-function, linear)
      calc(-1s * (100cqi / 1px - var(--min, 0)) / (var(--max, 0) - var(--min, 0)))
      1
      both
      paused
      var(--name, none);
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
```

This is the same code Typetura uses, except it uses JavaScript to attach `resizeObserver`s to container elements, passwing a `var(--width)` value into the animation delay function for better browser support.

## Key scenarios

### Reduce excessive breakpoints

Currently the following websites use numerous breakpoints to define how typesetting changes at different screen sizes:
[The Outline](https://theoutline.com/), [HEX](https://hex.xyz/Margo/) and [Elliot Jay Stocks](https://elliotjaystocks.com/).

### Consolodate styles

[The Atlantic](https://www.theatlantic.com/) has a typographically rich home page on desktop with headlines of various sizes based on their container size. All of these headline styles could be consolodated into one interpolated ruleset for this layout with the additional bonus of working across viewport sizes. Here is a demo of [The Atlantic with an interpolated headline style](https://demos.typetura.com/the-atlantic).

### Changing typesetting around layout shifts

The [Stripe](https://stripe.com/) home page changes it’s type styles for a layout shift. If these styles were interpolated and bound to the text container itself, then as the layout shift happens the text would automatically resize appropriately. Here is an example of [what this type of layout might look like with interpolated styles](https://demos.typetura.com/magazine.html).

### Smooth layout shifts on resize

Lynn Fisher used Typetura to interpolate rulesets with CSS for [her latest 2021 website redesign](https://lynnandtonic.com/). This would all be possible natively in CSS with ruleset interpolation. Additionally more subtle applications of this can be applied, like the way the floating images slide in and out of the text column where there is space on [this “Explore” demo](https://demos.typetura.com/magazine.html).

## Stakeholder Feedback / Opposition

- Chromium : Waiting on feedback
- Gecko : Waiting on feedback
- Webkit : Waiting on feedback

## References & acknowledgements

- Miriam Suzanne for proposal collaboration
- Nicole Sullivan for general advice and proposal feedback
- Google UI Fund for project support
- Ana Monroe for editing and design feedback

#### Additional thanks to support, feedback, and inspiration from

- Florian Schulz
- Jen Simmons
- Tim Brown

## Changelog

### 2022.10.31

- First draft