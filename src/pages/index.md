---
layout: /src/layouts/Index.astro
---

<!-- @format -->

# CSS Typographic Initiatives

This is the project space for the open web standards work of [Scott Kellum](https://scottkellum.com) and [Typetura](https://typetura.com).

Making typography on the web better is the reason why Typetura was founded, and improving the web platform as a whole is critical to achieving this goal. This project space is made possible by the generous support of the [Google UI Fund](https://web.dev/ui-fund/).

## [Ruleset interpolation](ruleset-interpolation)

The biggest tension in [responsive web design](glossary/#responsive-web-design) is the interaction between [flexible media and inflexible text](https://frankchimero.com/blog/2015/the-webs-grain/). In responsive layouts, text behaves like an incompressible liquid that expands in the [block](glossary/#block) direction as it is compressed in the [inline](glossary/#inline) direction. Conversely, flexible media shrinks in the block direction proportionally to shrinking in the inline direction.

Providing a toolset to interpolate styles as containers shrink will allow web authors to ensure text is well styled regardless of layout or device. Balancing [line length](glossary/#measure), [font size](glossary/#font-size), and [line height](glossary/#line-height) along with [variable font](glossary/#variable-font) settings like [optical size](glossary/#optical-size), [width](glossary/#width), and [weight](glossary/#weight).

### [Proposal](/ruleset-interpolation/proposal)

### [Roadmap](/ruleset-interpolation/roadmap)

### [Notes and references](/ruleset-interpolation/notes)

### [Examples](/ruleset-interpolation/examples)

---

## [Glossary](glossary)

Typographic terms can be confusing. The differences between terms like leading and line height, type design and typography, and typeface and font can dramatically change what is being communicated. It’s important to have clear and well documented definitions of the terms we use to ensure we are all on the same page.

## [Web typography audit](typography-audit)

There is a lot of work that has been done to make the web a better place for typography. There is also a lot of low-hanging fruit that needs to be implemented, is implemented and needs a clear spec, or where there is opportunity for iteration. This is a survey of these areas and where we might improve typography on the web platform incrementally.
