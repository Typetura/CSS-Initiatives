---
layout: /src/layouts/Default.astro
---

<!-- @format -->

# Problems and opportunities

Currently there is one way to fluidly scale text on the web, by using a `calc()` function in combination with viewport or container units. CSS authors love [using `calc()` for fluid text sizing](https://css-tricks.com/fun-tip-use-calc-to-change-the-height-of-a-hero-component/). However there are N major issues with `calc()`: the mental model is at odds with other responsive approaches, it’s a linear function, it only works with length typed units for now, the function can become complicated, and the function can only be applied to a single property at a time. The complexities of using `calc()` have resulted in numerous web based calculators to help CSS authors out like [Utopia](https://utopia.fyi/space/calculator) and this [clamp() calculator](https://www.marcbacon.com/tools/clamp-calculator/).
