---
layout: /src/layouts/Default.astro
---

# Glossary

Shared definitions are important to adequate communication. Typographic terms are often confusing or hold different definitions in different organizations. This is aimed to help clarify what words mean in the context of this documentation.

This glossary references [The Typographic Desk Reference](https://typedeskref.com/) by Theo Rosendorf and [Practical Typography](https://practicaltypography.com/) by Matthew Butterick.

---

### Baseline

The lower boundary on which text that is written horizontally sits. Some [glyphs](#glyph) may be drawn below this line, and these instances are called “descenders”. All Latin uppercase characters are drawn on and above this line.

### Block

The dimension that runs perpendicular to the flow of text. For Latinic languages, block is the vertical dimension.

### Em

A unit of measurement equal to the [font size](#font-size). Note that this unit might correlate with, but has nothing to do with the width of the “M”.

### Font

An artifact that enables replication and use of artwork or [glyphs](#glyph) from a [typeface](#typeface). A font can be a <abbr title="Web Open Font Format 2">WOFF2</abbr> or <abbr title="True Type Font">TTF</abbr> file, a photographic disc, or cast in metal.

### Font size

The total height of the design space allotted to a [glyph](#glyph). This is most easily visible with metal or wood [fonts](#font) where the letters exist on a physical block of a specific height. The anatomy of a digital font is similar, but this design space can be scaled up or down to suit the design needs.

### Glyph

A singular drawing of a character, letter, punctuation, emoji, or mark that is part of a [typeface](#typeface).

### Inline

The dimension that runs parallel to the flow of text. For Latinic languages, inline is the horizontal dimension.

### Italic

A style of typeface that is usually drawn with a [slant](#slant) and has notably more cursive stylings for the a(_a_), g(_g_), and e(_e_) letterforms. It was [originally designed by Aldus Manutius](https://books.google.com/books?id=6sidSDlif48C&pg=PA287#v=onepage&q&f=false) in Venice Italy to print smaller books.

### Leading

The space between lines of text. On the web, half of this space is placed above and half below the text. With lead type, strips of lead were placed between each line of text to add space, hence the term _“leading”_. 

**Note:** Some software refers to [line height](#line-height) as “leading”. This is _not_ the definition used in this documentation.

### Line height

The sum of the [font size](#font-size) and spacing between lines or [leading](#leading).

### Line length

The [inline](#inline) dimension of text.

Also referred to as _“measure”_.

### Metrics

The various guides within a digital [font](#font) file indicating the [inline](#inline) size of [glyphs](#glyphs) as well as the vertical dimensions like [baseline](#baseline), cap-height, and [x-height](#x-height).

### Optical size

The design of a [typeface](#typeface) may change as it is displayed large or small. For example, the [x-height](#x-height) may increase in smaller text and [contrast](#contrast) may reduce. This happens fluidly and automatically within [variable fonts](#variable-font) with this feature. With metal [fonts](#font), these different design characteristics are cast in the the metal of different sizes of the [typeface](#typeface).

### Slant

How slanted the letterforms are. This usually relates to the degrees away from 90°. This is different from [italics](#italic), which are drawn differently.

A font that has a slant without italic letterforms is called an _“oblique”_.

### Text

The raw material that is written content. This does not refer to the [typesetting](#typesetting), [typeface](#typeface) used, nor any other aspect of its styling.

### Type design

The practice of drawing [glyphs](#glyph) that are part of a collection or [typeface](#typeface).

### Typeface

A cohesive collection of forms known as [glyphs](#glyph) that can be re-ordered to convey meaning. The artifact or embodiment of a typeface is called a [font](#font).

### Typesetting

Mostly synonymous with [typography](#typography), but is focused on the systematic styling of [text](#text) as opposed to unique and expressive styling for specific [glyphs](#glyph), words, or phrases.

### Typography

The use and application of a [typeface](#typeface) in a design.

### Responsive web design

A website containing percent based grids, percent based media, and styles for various screen sizes. [The term was coined by Ethan Marcotte](https://alistapart.com/article/responsive-web-design/).

### Variable font

A [font](#font) that contains information enabling it to interpolate it’s form along one or multiple axis. The axis may include [**weight**](#weight), [_slant_](#oblique), [optical size](#optical-size), [width](#width), or any number of arbitrary axis. Variable fonts may be authored with [multiple masters](https://glyphsapp.com/learn/multiple-masters-part-1-setting-up-masters), [parameters](https://material.io/blog/roboto-flex), or [higher level interpolation](https://www.underware.nl/case-studies/hoi/).

The official name is “OpenType Font Variations”

### Weight

The average width and height of strokes of a [glyph](#glyph). This value is subjective and refers to the visual weight text has.

### Width

This is referred to as `font-stretch` in CSS. It’s an arbitrary scale that defines how wide or narrow the [glyphs](#glyph) are in a font. A narrow width is often called “compressed”, “condensed”, or “narrow” while a wide width is often called “expanded”, “extended”, or “wide”.

### X-height

The general height of the lowercase characters. This is represented as a guideline in the [metrics](#metrics) of the digital [font](#font) file itself.