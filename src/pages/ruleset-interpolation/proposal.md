---
layout: /src/layouts/Default.astro
---

<!-- @format -->

# Ruleset interpolation proposal

There should be a way to bind keyframe animations to the size of containers, similar to how `scroll-timeline` binds animations to scroll depth. This would give authors more fluid control over how their websites and components scale with fewer breakpoints.

There are two possible paths this specification might take. We already have container queries and attaching an animation to that container might be a good way to keep styles consistent. The container could also be defined using a similar syntax to `@scroll-timeline`.

## Animation attachment to a container

```css
article {
  container-type: inline-size;
  container-name: article;
}

.headline {
  /* Set the animation timeline to start at a conatainer inline-size of 20rem and end at 80rem */
  /* attahment function ( container name and dimention, start size, end size ) */
  animation-attachment: container(article inline-size, 20rem, 80rem);

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
    font-size: 1.6rem;
    color: hsl(9, 69%, 11%)
  }
  to {
    transform: 5rem;
    line-height: 1;
    color: hsl(9, 96%, 29%)
  }
}
```

## Animation timeline

```css
/* Set the animation timeline to start at a conatainer inline-size of 20rem and end at 80rem */
@query-timeline article {
  source: selector("article");
  orientation: "inline";
  length-offsets: 20rem 80rem;
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
    font-size: 1.6rem;
    color: hsl(9, 69%, 11%)
  }
  to {
    transform: 5rem;
    line-height: 1;
    color: hsl(9, 96%, 29%)
  }
}
```

## Todo

- Decide if we should register the timeline container with the `@query-timeline` property similar to animation timelines or using `container-type` to align with the container query specification.
