# Typographic CSS Audit

## Has spec but not consistently implemented

This is a prioritized list of CSS modules that have been specced but are not implemented consistently across browsers.

1. [CSS Unit division](https://drafts.csswg.org/css-values-4/#changes-recent)
    - This isn’t explicitly a typographic feature, but this is a big step towards enabling fluid variable font settings and font animations on resize. An incremental step towards my [initial proposal](/proposal.md).
    - Lea Verou discusses the need for this in her [recent talk](https://youtu.be/ZuZizqDF4q8?t=2235).
1. [Text-wrap: balance](https://www.w3.org/TR/css-text-4/#valdef-text-wrap-balance)
    - The NYTimes has a [popular text balancer](https://github.com/nytimes/text-balancer). At Vox Media we inserted non-breaking spaces at the end of headlines to avoid egregiously unbalanced text. Having this built into browsers would make headlines look much better across the web.
1. [Text-justify](https://developer.mozilla.org/en-US/docs/Web/CSS/text-justify)*
    - I suggest adding a flexbox-style part to this spec where one can control how much word spacing, letter spacing, and font stretch go into the algorithm.
    - [Issue on GitHub](https://github.com/w3c/csswg-drafts/issues/7738)
1. [Hyphens](https://developer.mozilla.org/en-US/docs/Web/CSS/hyphens)*
    - Currently this is English only and requires a prefix.
1. [CSS Line Grid](https://www.w3.org/TR/css-line-grid-1/)
    - Baseline grids in CSS are helpful to keep text across multiple columns cleanly aligned.
    - Note [this issue with the headline examples in the spec](https://github.com/w3c/csswg-drafts/issues/6025).
1. [Initial Letters](https://www.w3.org/TR/css-inline/#initial-letter-styling)
    - ~~If I were to add something here, it would be padding of n lines of padding around an initial letter.~~
    - ~~[Issue on Github](https://github.com/w3c/csswg-drafts/issues/7739)~~
1. [Leading/trailing trim](https://css-tricks.com/leading-trim-the-future-of-digital-typesetting/)
    - Helpful to align the first or last line of text with the container or other content on the page.

*`text-justify` and `hyphens` are blocking `text-align: justify` from being acceptable to use.

## Implemented but needs a spec

- `-webkit-background-clip`: implemented in all major browsers but doesn’t have a spec. [Webkit’s blog post on background-clip: text](https://webkit.org/blog/164/background-clip-text/).
- `-webkit-text-stroke`: implemented in all major browsers but doesn’t have an official spec. [The -webkit-text-stroke compatibility standard can be found here](https://compat.spec.whatwg.org/#the-webkit-text-stroke).
    - When codifying into an official spec, please [note the issues with outline overlaps that are common in variable fonts](https://github.com/google/fonts/issues/4212).

## Open issues

- [Interpolation between breakpoints](https://github.com/w3c/csswg-drafts/issues/6245) (fluid type)
- [Query page zoom to compensate for text sizing adjustments](https://github.com/w3c/csswg-drafts/issues/6869) (Accessibility with fluid type)
- [Control justification settings](https://github.com/w3c/csswg-drafts/issues/7738)
- ~~[Initial letter padding](https://github.com/w3c/csswg-drafts/issues/7739)~~
