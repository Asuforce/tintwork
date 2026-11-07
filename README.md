# tintwork

Design tokens, base styles and web fonts shared across asuforce sites. Plain CSS, no build step.

```text
tintwork/
├── tokens.css   # :root custom properties: color (dark and light), font stacks, layout, motion
├── base.css     # reset, body, selection, focus ring, skip link, .container
├── fonts.css    # @font-face for the bundled fonts
└── fonts/       # woff2 files with each family's OFL.txt
```

## Use

Vite or Astro, from a git tag:

```sh
npm install github:Asuforce/tintwork#v0.1.1
```

```js
import '@asuforce/tintwork/fonts.css'
import '@asuforce/tintwork/tokens.css'
import '@asuforce/tintwork/base.css'
```

Without npm, copy the three CSS files and `fonts/` from a tag into your project and keep the relative paths between `fonts.css` and `fonts/`.

Components read the tokens with `var(--…)`, so a site keeps its own layout and page styles on top of these files.

## Tokens

Dark is the default. `:root[data-theme="light"]` overrides the colors that change.

| Group | Tokens |
|---|---|
| Surface | `--bg`, `--surface`, `--rule` |
| Text | `--text`, `--text-soft`, `--muted` |
| Accent | `--accent`, `--on-accent`, `--accent-text`, `--focus`, `--thumb` |
| Font | `--font`, `--display`, `--mono` |
| Layout | `--nav-h`, `--gutter` |
| Motion | `--ease` |

`--accent` is a neon fill and reads poorly on the light background, so use `--accent-text` when the accent colors text.

The theme attribute is set by the site. tintwork ships no script.

The font stacks end with Japanese fallbacks because none of the bundled fonts cover Japanese glyphs.

## Fonts

The files in `fonts/` are the unmodified woff2 files from each upstream, with the upstream license text next to them.

| Family | Weights | License | Source |
|---|---|---|---|
| D-DIN | 700 | OFL 1.1 | https://github.com/amcchord/datto-d-din |
| Archivo | 300, 400 | OFL 1.1 | https://github.com/Omnibus-Type/Archivo |
| JetBrains Mono | 400 | OFL 1.1 | https://github.com/JetBrains/JetBrainsMono |

D-DIN has the reserved font name "D-DIN". Do not convert, subset or rename the file.

## License

The CSS is MIT (see `LICENSE`). The fonts keep their own licenses, found in `fonts/<family>/OFL.txt`.

See `docs/DESIGN.md` for the design notes (Japanese).

## Optional navigation

Import `@asuforce/tintwork/components/navigation.css` after tokens and base.
Existing sites are unaffected until they add the classes.

```text
tintwork: surface → items → link states
site:     position → breakpoint → current section → animation
```

```html
<nav class="tw-nav tw-nav--clear" aria-label="Main">
  <ul class="tw-nav__items">
    <li><a class="tw-nav__link" href="/" aria-current="page">Home</a></li>
    <li><a class="tw-nav__link" href="/settings">Settings</a></li>
  </ul>
</nav>
```

Use `tw-nav--column` for vertical navigation or `tw-nav--bottom` for a
horizontal bottom bar. These modifiers do not set fixed positioning or
breakpoints. The site owns responsive layout, safe-area insets, content
clearance, theme selection and `aria-current` updates (`page` for routes,
`location` for sections; omit it on other links).

The optional stylesheet defines `--nav-*` surface and state tokens for dark
and light themes. Current links use the existing accent text token; hover
uses blue and keyboard focus uses `--focus`. Clear surfaces fall back to an
opaque background without backdrop filtering. Override tokens on the nav
when a site needs another surface. Keep long labels readable by selecting
an appropriate layout rather than shrinking them.
