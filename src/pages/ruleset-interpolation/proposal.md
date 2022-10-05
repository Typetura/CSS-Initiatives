---
layout: /src/layouts/Default.astro
---

<!-- @format -->

# Ruleset interpolation proposal

There should be a way to bind keyframe animations to the size of containers, similar to how `scroll-timeline` binds animations to scroll depth. This would give authors more fluid control over how their websites and components scale with fewer breakpoints.

There are two possible paths this specification might take. We already have container queries and attaching an animation to that container might be a good way to keep styles consistent. The container could also be defined using a similar syntax to `@scroll-timeline`.

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

## Animation attachment to a container

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

## Todo

- Decide if we should register the timeline container with the `@query-timeline` property similar to animation timelines or using `container-type` to align with the container query specification.
