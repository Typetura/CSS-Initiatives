---
layout: /src/layouts/Default.astro
---

<!-- @format -->

# Problems and opportunities

Currently there is one way to fluidly scale text on the web, by using a `calc()` function in combination with viewport or container units. CSS authors love [using `calc()` for fluid text sizing](https://css-tricks.com/fun-tip-use-calc-to-change-the-height-of-a-hero-component/). However there are N major issues with `calc()`: the mental model is at odds with other responsive approaches, it’s a linear function, it only works with length typed units for now, the function can become complicated, and the function can only be applied to a single property at a time. The complexities of using `calc()` have resulted in numerous web based calculators to help CSS authors out like [Utopia](https://utopia.fyi/space/calculator) and this [clamp() calculator](https://www.marcbacon.com/tools/clamp-calculator/).

## The issues with `calc()`

Of the [six desired features needed in ruleset interpolation](proposal/#desired-features), `calc()` and `clamp()` can solve three of the points but fail to address the ability to change interpolation easing, allowing for multiple interpolation points, and the interpolation of multiple properties at once. Of the three remaining features `calc()` and `clamp()` can support, the math required to interpolate all unit types is verbose and the mental model for defining start and end points with clamp often requires the use of an online calculator.

### How `calc()` and `clamp()` do regarding the six desired features of ruleset interpolation:

| `Calc()` and `clamp()` support | Feature                                                      |
| ------------------------------ | ------------------------------------------------------------ |
| ✅                              | The ability to interpolate styles across container and viewport widths |
| ⚠️                              | This interpolation must support all CSS interpolate types including `length`, `color`, and `number` |
| ❌                              | There needs to be the ability to define the easing of the interpolation |
| ⚠️                              | The ability to define the start and end size of the interpolation |
| ❌                              | More than two points can be interpolated between. For example an inline-size of 20rem, 40rem, and 80rem can have different rulesets |
| ❌                              | Multiple properties can be interpolated at once. It’s rare that I need only one `property: value` pair to change at a time |

### Defining start and end points with `clamp()`

Clamp is a fantastic tool for defining minimum and maximum values, but this is at odds with the model web authors use when creating websites. Breakpoints in media queries set up the model where a particular viewport or component size is an anchor for design characteristics. As a result, most web authors use calculators that generate a `clamp()` function from start and end points. When using relative units like `em` and `rem`, this function can break alignment with other breakpoints on a page when the browser default font size is changed to a value other than `16px`.

Ideally we would have a solution that works with the model of defining at what widths interpolation starts and ends along with corresponding desired values.

### Non-length based units

The `calc()` function to interpolate values over a viewport or container width is defined with length based values like so: `calc(0.8rem + 3vw)`. CSS does support interpolating non-length based values across widths but it makes this calculation far less intuitive: `font-weight: clamp(1, 30vw / 1px + 100, 900)` or `color: hsl(10deg 100% clamp(0%, 10vw / 1px * 1% -10%, 50%))`. Note that while this is all valid CSS, it is not supported by any browser at the time of writing this.
