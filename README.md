<br /> <br /> <br /> <br />

<div align="center">
  <img src=".github/logo.svg" alt="ssbuild logo" width="300" />
</div>

<br /> <br />

<p align="center">
  A style system generator and utility-first CSS framework built in Sass.
</p>

<p align="center">
  <a aria-label="test status" href="https://github.com/maurandco/ssbuild/actions/workflows/test.yml">
    <img alt="test" src="https://img.shields.io/github/workflow/status/maurandco/ssbuild/test.yml?label=test&style=flat-square" />
  </a>

  <a aria-label="version status" href="https://github.com/maurandco/ssbuild/releases">
    <img alt="version" src="https://img.shields.io/github/v/release/maurandco/ssbuild?display_name=tag&style=flat-square&color=B85A8A" />
  </a>
</p>

<br /> <br /> <br /> <br />

⚠️ **This project is still under active development. Check back soon for a release!**

## Features

- Highly customizable style system.
- Resonsive-first & utility-first classes.
- Beautiful defaults for color, shadow, scale, and typography.
- Support for dark mode utility classes and media queries.
- Stacked pseudo selector classes for precision styling.
- Automatic color scale generator for custom colors.
- Easily extended with mixins and custom Sass.
- [Add or remove classes from build as needed.](#generators)
<!-- - 0kB gzipped and 0kB brotli'd for default build. -->

## Why?

ssbuild was inspired by similar utility-first CSS frameworks such as TailwindCSS and Tachyons, but it comes with the power of a CSS preprocessor and zero dependencies other than [dart-sass](https://sass-lang.com/dart-sass) (which is fast and self-contained). As opposed to dealing with a Node build system and PostCSS with its plethora of plugins.

## Installation

If you don't need any customization and want to start using the utility classes right away, you can link directly to the default build in your HTML.

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/ssbuild@0.1/index.min.css" />
```

If you want to customize your build, you'll need to install ssbuild into your project using one of these methods.

- [Download the latest release](https://github.com/maurandco/ssbuild/releases)
- Install with [hex](https://hex.pm) (Elixir): _not supported yet_
- Install with [gem](https://rubygems.org) (Ruby): _not supported yet_
- Install with [pip](https://pypi.org) (Python): _not supported yet_
- Install with [npm](https://www.npmjs.com) (Node): _not supported yet_

## Usage

```scss
@use "ssbuild";
```

That's it! All the utility classes will now be included in your CSS output.

```html
<div class="measure-narrow bg-gray-1">
  <h1 class="text-xxl text-heavy pink-5 mb-4">
    Welcome to ssbuild!
  </h1>

  <p class="text-sm text-light">
    Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt
    ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation
    ullamco laboris nisi ut aliquip ex ea commodo consequat.
  </p>
</div>
```

## Customization

### Example

```sass
@use "ssbuild" with (
  $normalize: false,
  $dark-mode: class,
  $primary-fonts: (
    sans-serif: "Fira Sans",
    monospace: "Fira Mono",
  ),
  $extend: (
    colors: (
      off-black: #202020,
    ),
  ),
);
```

All custom values will **override the defaults** unless they are configured within the [$extend](#extend) map.

### Options

- [$normalize](#normalize)
- [$class-prefix](#class-prefix)
- [$separator](#separator)
- [$dark-mode](#dark-mode)
- [$screens](#screens)
- [$measures](#measures)
- [$spacing](#spacing)
- [$primary-fonts](#primary-fonts)
- [$font-stacks](#font-stacks)
- [$font-weights](#font-weights)
- [$font-sizes](#font-sizes)
- [$font-leading](#font-leading)
- [$font-tracking](#font-tracking)
- [$text-indent](#text-indent)
- [$box-shadows](#box-shadows)
- [$colors](#colors)
- [$auto-colors](#auto-colors)
- [$selectors](#selectors)
- [$generators](#generators)
- [$extend](#extend)

### Normalize

Whether to import [modern-normalize](https://github.com/sindresorhus/modern-normalize) and some other [opinionated normalize rules](lib/normalize.scss) at the top of your stylesheet.

> type: `boolean`  
> default: `true`

### Class Prefix

Used to prefix a value to every generated class name. This is turned off by default, so classes will look like `.bg-black`. e.g. If set to `ss-`, all the classes generated will look like `.ss-bg-black`.

> type: `string`  
> default: none

### Separator

Used to separate base class names from variants such as dark selectors, pseudo selectors, and responsive selectors. Using the default value of an escaped colon will generate classes following the pattern of `.sm\:dark\:bg-black`.

> type: `string`  
> default: `\\:`

### Dark Mode

Enables dark mode classes using media queries or a parent class. Setting to `media` will create classes within `@media (prefers-color-scheme: dark)`, and setting to `class` will create classes under a parent `.dark` class such as `.dark .dark\:bg-black`.

> type: `string`  
> default: `media`

### Screens

Named screen size values which are used to generate responsive mixins and classes such as `.md\:bg-black`.

> type: `map`  
> default:
> ```sass
> (
>   xs: 640px,
>   sm: 768px,
>   md: 1024px,
>   lg: 1280px,
>   xl: 1792px,
> )
> ```

### Measures

Named width values used for paragraphs and other sized containers. These classes are prefixed with `.measure-*`. See the [max-width generator](lib/generators/max-width#readme) for more info.

> type: `map`  
> default:
> ```sass
> (
>   narrow: 20em,
>   regular: 30em,
>   wide: 34em,
> )
> ```

### Spacing

Spacing values used for margin and padding. Class names correspond with index in list starting at 1. e.g. `.m-1` will correspond to `margin: .25rem`, and `.m-8` will correspond to `margin: 32rem`. These classes are prefixed with `.m-*` and `.p-*`. See the [margin generator](lib/generators/margin#readme) and [padding generator](lib/generators/padding#readme) for more info.

> type: `list`  
> default:
> ```sass
> [
>   .25rem,
>   .5rem,
>   1rem,
>   2rem,
>   4rem,
>   8rem,
>   16rem,
>   32rem,
> ]
> ```

### Primary Fonts

A primary font can be set for each type of font stack, such as `monospace: "Fira Mono"`, which prepends to the default font stack of that same type. This avoids having to redefine entire system font stacks from scratch just to add a custom font.

> type: `map`  
> default:
> ```sass
> (
>   serif: null,
>   sans-serif: null,
>   monospace: null,
> )
> ```

### Font Stacks

Named list values for font family stacks. Rather than redefining entire font stacks to specify a single custom font, it's recommended to specify a [primary font](#primary-fonts) instead. These classes **do not** have a prefix. See the [font-family generator](lib/generators/font-family#readme) for more info.

> type: `map`  
> default:
> ```sass
> (
>   serif: [
>     ui-serif,
>     Georgia,
>     Cambria,
>     "Times New Roman",
>     Times,
>     serif,
>     "Apple Color Emoji",
>     "Segoe UI Emoji",
>     "Segoe UI Symbol",
>     "Noto Color Emoji",
>   ],
>   sans-serif: [
>     ui-sans-serif,
>     system-ui,
>     -apple-system,
>     BlinkMacSystemFont,
>     "Segoe UI",
>     Roboto,
>     "Helvetica Neue",
>     Arial,
>     "Noto Sans",
>     sans-serif,
>     "Apple Color Emoji",
>     "Segoe UI Emoji",
>     "Segoe UI Symbol",
>     "Noto Color Emoji",
>   ],
>   monospace: [
>     ui-monospace,
>     SFMono-Regular,
>     Menlo,
>     Monaco,
>     Consolas,
>     "Liberation Mono",
>     "Courier New",
>     monospace,
>     "Apple Color Emoji",
>     "Segoe UI Emoji",
>     "Segoe UI Symbol",
>     "Noto Color Emoji",
>   ],
> )
> ```

### Font Weights

Named values used for font weight. These classes are prefixed with `.text-*`. See the [font-weight generator](lib/generators/font-weight#readme) for more info.

> type: `map`  
> default:
> ```sass
> (
>   thin: 100,
>   extra-light: 200,
>   light: 300,
>   regular: 400,
>   medium: 500,
>   semi-bold: 600,
>   bold: 700,
>   extra-bold: 800,
>   heavy: 900,
> )
> ```

### Font Sizes

Named values used for font size. These classes are prefixed with `.text-*`. See the [font-size generator](lib/generators/font-size#readme) for more info.

> type: `map`  
> default:
> ```sass
> (
>   xxs: .75rem,
>   xs: .875rem,
>   sm: 1rem,
>   md: 1.25rem,
>   lg: 1.5rem,
>   xl: 2.25rem,
>   xxl: 3rem,
>   jumbo: 6rem,
> )
> ```

### Font Leading

Named values used for font leading (which is line height). These classes are prefixed with `.lead-*`. See the [line-height generator](lib/generators/line-height#readme) for more info.

> type: `map`  
> default:
> ```sass
> (
>   solid: 1em,
>   title: 1.25em,
>   copy: 1.5em,
> )
> ```

### Font Tracking

Named values used for font tracking (which is letter spacing). These classes are prefixed with `.track-*`. See the [letter-spacing generator](lib/generators/letter-spacing#readme) for more info.

> type: `map`  
> default:
> ```sass
> (
>   tight: -.05em,
>   open: .1em,
>   mega: .25em,
> )
> ```

### Text Indent

Value used for the text indentation class. See the [text-indent generator](lib/generators/text-indent#readme) for more info.

> type: `number`  
> default: `1em`

### Box Shadows

Named values used for box shadow. These classes are prefixed with `.shadow-*`. See the [box-shadow generator](lib/generators/box-shadow#readme) for more info.

> type: `map`  
> default:
> ```sass
> (
>   low: (0 4px 6px -1px rgb(0 0 0 / 0.1), 0 2px 4px -2px rgb(0 0 0 / 0.1)),
>   mid: (0 10px 15px -3px rgb(0 0 0 / 0.1), 0 4px 6px -4px rgb(0 0 0 / 0.1)),
>   high: (0 20px 25px -5px rgb(0 0 0 / 0.1), 0 8px 10px -6px rgb(0 0 0 / 0.1)),
>   mega: 0 25px 50px -12px rgb(0 0 0 / 0.25),
>   inner: inset 0 2px 4px 0 rgb(0 0 0 / 0.05),
> )
> ```

### Colors

Named values for color and background color. These classes **do not** have a prefix, e.g. `black` creates a `.black` class. See the [color generator](lib/generators/color#readme) and [background-color generator](lib/generators/background-color#readme) for more info.

> type: `map`  
> default:
> ```sass
> (
>   black: #000000,
>   white: #ffffff,
> )
> ```

### Auto Colors

Named values for colors that need auto-generated variations. ssbuild will generate 4 darker variations and 4 lighter variations of each color, for a total of 9. These classes are prefixed with the color name. The base color will exist as `.[name]-5` with `.[name]-[1-4]` for darker variations and `.[name]-[6-9]` for lighter variations. See the [color generator](lib/generators/color#readme) and [background-color generator](lib/generators/background-color#readme) for more info.

> type: `map`  
> default:
> ```sass
> (
>   gray: #adb5bd,
>   cyan: #0dcaf0,
>   teal: #20c997,
>   green: #198754,
>   yellow: #ffc107,
>   orange: #fd7e14,
>   red: #dc3545,
>   pink: #d63384,
>   purple: #6f42c1,
>   indigo: #6610f2,
>   blue: #0d6efd,
> )
> ```

### Selectors

All the selectors that can be specified in the [generators](#generators), and the associated suffix for the classes. e.g. If the `first` selector is enabled for a generator, it will create classes such as `.first\:bg-black:first-child`.

The selector name can also be a space-separated list for nested selectors. e.g. `hover first: ":hover:first-child"` will create `.hover\:first\:classname:hover:first-child`. Using a space in the key name is to ensure the configured `$separator` is used in the final class.

> type: `map`  
> default:
> ```sass
> (
>   // pseudo classes
>   hover: ":hover",
>   focus: ":focus",
>   focus-within: ":focus-within",
>   focus-visible: ":focus-visible",
>   active: ":active",
>   visited: ":visited",
>   target: ":target",
>   first: ":first-child",
>   last: ":last-child",
>   only: ":only-child",
>   odd: ":nth-child(odd)",
>   even: ":nth-child(even)",
>   first-of-type: ":first-of-type",
>   last-of-type: ":last-of-type",
>   only-of-type: ":only-of-type",
>   empty: ":empty",
>   disabled: ":disabled",
>   checked: ":checked",
>   indeterminate: ":indeterminate",
>   default: ":default",
>   required: ":required",
>   valid: ":valid",
>   invalid: ":invalid",
>   in-range: ":in-range",
>   out-of-range: ":out-of-range",
>   placeholder-shown: ":placeholder-shown",
>   autofill: ":autofill",
>   read-only: ":read-only",
>   rtl: ":dir(rtl)",
>   ltr: ":dir(ltr)",
>   // pseudo elements
>   before: "::before",
>   after: "::after",
>   first-letter: "::first-letter",
>   first-line: "::first-line",
>   marker: "::marker",
>   selection: "::selection",
>   file: "::file-selector-button",
>   placeholder: "::placeholder",
>   // attribute selectors
>   open: "[open]",
>   // nested selectors
>   hover disabled: ":hover:disabled",
>   hover before: ":hover::before",
>   hover after: ":hover::after",
> )
> ```

### Generators

Rather than include every single possible utility class in the final CSS (which would result in an enormous file), we added the ability to add or remove specific classes in the config, which we call "generators". Not using dark mode? Set the generators to an empty list. Want to add utility classes for a specific pseudo selector, or even nested pseudo selectors? Add them to the generators list.

This is different than other systems like Tailwind, which uses a just-in-time method where a file watcher scans your HTML files for classes being used and includes them in the final CSS. We decided to leave that up to the configuration. It's a bit more work on the developer side to manually include the class variations they need, but it's also more predicable.

See [selectors](#selectors) for a list of available selectors. Use the `dark` selector to support dark classes, and the `responsive` selector or individual screen selectors (such as `sm` and `md`) for responsive classes. Responsive dark classes can be enabled with the nested `dark` selector within `responsive` or the individual screen selectors. To avoid copying the same lists over and over if multiple selectors are used, you can use a Sass variable.

> type: `map`  
> default:
> ```sass
> (
>   base: [
>     background-color box-shadow box-sizing color font-family font-size font-style font-variant
>     font-weight letter-spacing line-height margin max-width opacity padding text-align
>     text-decoration text-indent text-overflow text-transform vertical-align white-space width
>   ],
>   hover: [],
>   dark: (
>     base: [],
>   ),
>   responsive: (
>     base: [],
>     dark: (
>       base: [],
>     ),
>   ),
> )
> ```

[See all the available generators.](lib/generators#readme)

### Extend

This can be used to append custom values onto config maps without replacing the existing values. For example, if you'd like to add a new font size called `huge` while keeping the original font sizes:

```sass
$extend: (
  font-sizes: (
    huge: 10rem,
  ),
)
```

> type: `map`  
> default: none
