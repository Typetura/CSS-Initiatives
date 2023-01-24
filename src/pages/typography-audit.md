---
layout: /src/layouts/Default.astro
---

<!-- @format -->

# Typographic CSS audit & wish list

## Has spec but not consistently implemented

This is a prioritized list of CSS modules that have been specced but are not implemented consistently across browsers.

1. [CSS Unit division](https://drafts.csswg.org/css-values-4/#changes-recent)
   - This isn’t explicitly a typographic feature, but this is a big step towards enabling fluid variable font settings and font animations on resize. This is an important part of [the roadmap](/ruleset-interpolation/roadmap).
   - Lea Verou discusses the need for this in her [recent talk](https://youtu.be/ZuZizqDF4q8?t=2235).
1. [Text-wrap: balance](https://www.w3.org/TR/css-text-4/#valdef-text-wrap-balance)
   - The NYTimes has a [popular text balancer](https://github.com/nytimes/text-balancer). At Vox Media we inserted non-breaking spaces at the end of headlines to avoid egregiously unbalanced text. Having this built into browsers would make headlines look much better across the web.
1. [Text-justify](https://developer.mozilla.org/en-US/docs/Web/CSS/text-justify)\*
   - I suggest adding a flexbox-style part to this spec where one can control how much word spacing, letter spacing, and font stretch go into the algorithm.
   - [Issue on GitHub](https://github.com/w3c/csswg-drafts/issues/7738)
1. [Hyphens](https://developer.mozilla.org/en-US/docs/Web/CSS/hyphens)\*
   - Currently this is English only and requires a prefix.
1. [CSS Line Grid](https://www.w3.org/TR/css-line-grid-1/)
   - Baseline grids in CSS are helpful to keep text across multiple columns cleanly aligned.
   - Note [this issue with the headline examples in the spec](https://github.com/w3c/csswg-drafts/issues/6025).
1. [Initial Letters](https://www.w3.org/TR/css-inline/#initial-letter-styling)
1. [Leading trim](https://css-tricks.com/leading-trim-the-future-of-digital-typesetting/)
   - Helpful to align the first or last line of text with the container or other content on the page.

\*`text-justify` and `hyphens` are blocking `text-align: justify` from being acceptable to use.

## Implemented but needs a spec

- `-webkit-background-clip`: implemented in all major browsers but doesn’t have a spec. [Webkit’s blog post on background-clip: text](https://webkit.org/blog/164/background-clip-text/).
- `-webkit-text-stroke`: implemented in all major browsers but doesn’t have an official spec. [The -webkit-text-stroke compatibility standard can be found here](https://compat.spec.whatwg.org/#the-webkit-text-stroke).
  - When codifying into an official spec, please [note the issues with outline overlaps that are common in variable fonts](https://github.com/google/fonts/issues/4212).

## Wish list

1. [Ruleset interpolation](/ruleset-interpolation)

2. Set font-size by [x-height](/glossary/#x-height)

   - [X-height](/glossary/#x-height) has more of an impact on the appearance of perceived font-size than the [font size](/glossary/#font-size) itself, which is the size of the art-space the font itself is drawn on.  _Note some considerations will need to be made regarding internationalization here as only alphabets with lowercase character sets have an x-height._

     [![A comparison of font-size between two different pieces of text.](/images/font-size-ex-height-example.png)](https://codepen.io/scottkellum/pen/zYjgRpZ)

   - Also [see this thread between Chris Armstrong and Una on Twitter](https://twitter.com/Armstrong/status/1583458332681338880), Chris created this [fantastic video describing the need for x-height based font sizing in detail](https://twitter.com/Armstrong/status/1583555898047201280).
   - [Myndex’s issue on font-sizing by x-height](https://github.com/w3c/csswg-drafts/issues/6709).

## [Research board](https://arc.net/e/6050A8B9-2B0C-44AB-A911-6F12E4862031)

## Open issues

- [Interpolation between breakpoints](https://github.com/w3c/csswg-drafts/issues/6245) (fluid type)
- [Query page zoom to compensate for text sizing adjustments](https://github.com/w3c/csswg-drafts/issues/6869) (Accessibility with fluid type)
- [Control justification settings](https://github.com/w3c/csswg-drafts/issues/7738)
- [Myndex’s issue on font-sizing by x-height](https://github.com/w3c/csswg-drafts/issues/6709)
