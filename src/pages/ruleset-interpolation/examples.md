---
layout: /src/layouts/Default.astro
---

<!-- @format -->

# Ruleset interpolation examples

- [Gold’s Guide](https://goldsguide.com/) — A production website where the entire typographic system is based on ruleset interpolation.
- [Explore demo](https://demos.typetura.com/magazine.html) — A demo page where the entire typographic system is based on ruleset interpolation. This demo shows how flexible these typographic systems are, using a single interpolated ruleset to scale much smaller than a phone and up to the size of a large monitor.
- [The Atlantic demo](https://demos.typetura.com/the-atlantic) — The top part of this page was reverse-engineered so that all the headlines are based on one single interpolated ruleset where before they were each styled individually. This requires complex scaling on a curve and is impossible to achieve with a `clamp()` function.
- [Nike demo](https://demos.typetura.com/nike.html) — This demo again consolidates all heading styles into a single interpolated ruleset, eliminating the need for multiple styles for each header at each screen size.
