---
layout: /src/layouts/Default.astro
---

<!-- @format -->

# Ruleset interpolation proposal

There should be a way to bind keyframe animations to the size of containers, similar to how `scroll-timeline` binds animations to scroll depth. This would give authors more fluid control over how their websites and components scale with fewer breakpoints.

There are two possible paths this specification might take. We already have container queries and attaching an animation to that container might be a good way to keep styles consistent. The container could also be defined using a similar syntax to `@scroll-timeline`.

## Desired features

- The ability to interpolate styles across container and viewport widths.
- This interpolation must support all CSS interpolate types including `length`, `color`, and `number`.
- There needs to be the ability to define the easing of the interpolation.
- The ability to define the start and end size of the interpolation.
- Ideally more than two points can be interpolated between. For example an inline-size of 20rem, 40rem, and 80rem can have different rulesets.
- Ideally multiple properties can be interpolated at once. It’s rare that I need only one `property: value` pair to change at a time.

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

## Container-animation

This proposal creates a more specific property focused on container interpolation as opposed to relying on animation. This sheds the baggage of time-based logic enabling `container-keyframes` to contain length based units. This proposal was [originally discussed in this gist](https://gist.github.com/scottkellum/0c29c4722394c72d311c5045a30398e5).

### Advantages

- A simple syntax
- Relies on container query syntax and mental model
- Length units can be used in keyframes
  - These units can also be mixed, so things can be interpolated from a size of 100px to a size of 30rem. As one example this may be useful in interpolating an icon’s appearance from its minimum pixel-based size to a size based on the text column it sits within.

- Does not require duration in order to work
- Can assume an `animation-fill-mode` of `both` as default

### Disadvantages

- A whole new spec to maintain as opposed to expanding the existing animation spec.

```css
article {
  container-type: inline-size;
  container-name: article;
}

.headline {
  /* Define the container animation parameters */
  container-interpolation-name: headline;
  container-interpolation-timing-function: ease-in-out;
}

@container-keyframes headline (inline-size) {
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

## Animation attachment to a container

This is an extension of `animation` to allow for binding to container queries. A new function here defines the what dimension is being queried as well as the start and end points of the interpolation.

### Advantages

- Relies on container query syntax and mental model
- Relies on animation and keyframe syntax and mental model
- Aligned with animation spec for future changes and maintenance

### Disadvantages

- Doesn’t allow for length based units in keyframes
- Has time-based baggage from animations and duration needs to be defined
- More verbose than [container-animation](#container-animation)
- `fill-mode` default of `none` is not ideal

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

## Animation timeline

This direction is more aligned with scroll-timeline. It is designed to mirror that spec, changing the syntax to `@query-timeline` as opposed to `@scroll-timeline` to define the query interpolation behavior.

### Advantages

- Relies on scroll-timeline syntax and mental model
- Relies on animation and keyframe syntax and mental model
- Aligned with animation spec for future changes and maintenance

### Disadvantages

- Doesn’t allow for length based units in keyframes
- Has time-based baggage from animations and duration needs to be defined
- More verbose than [container-animation](#container-animation)
- `fill-mode` default of `none` is not ideal

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
