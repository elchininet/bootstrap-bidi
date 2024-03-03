---
layout: docs
title: RTL
description: Learn about right-to-left text in Bootstrap across our layout, components, and utilities.
group: getting-started
toc: true
---

## Get familiar

We recommend getting familiar with Bootstrap first by reading through our [Getting Started Introduction page]({{< docsref "/getting-started/introduction" >}}).

You may also want to read up on [the PostCSS RTLCSS project](https://github.com/elchininet/postcss-rtlcss) (which uses [RTLCSS](https://rtlcss.com/) behind the scenes), as it powers our approach to RTL.

{{< callout warning >}}
**Bootstrap's RTL feature is still experimental** and will evolve based on user feedback. Spotted something or have an improvement to suggest? [Open an issue]({{< param repo >}}/issues/new/choose), we'd love to get your insights.
{{< /callout >}}

## Required HTML

There are two strict requirements for enabling RTL in Bootstrap-powered pages.

1. Set `dir="rtl"` on the `<html>` element.
2. Add an appropriate `lang` attribute, like `lang="ar"`, on the `<html>` element.

From there, you'll need to include the bootstrao CSS. For example, here's the stylesheet for our compiled and minified CSS:

```html
<link rel="stylesheet" href="{{< param "cdn.css" >}}" integrity="{{< param "cdn.css_hash" >}}" crossorigin="anonymous">
```

After that, it should automatically render the styles using the RTL version.

### Starter template

You can see the above requirements reflected in this modified RTL starter template.

```html
<!doctype html>
<html lang="ar" dir="rtl">
  <head>
    <!-- Required meta tags -->
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">

    <!-- Bootstrap CSS -->
    <link rel="stylesheet" href="{{< param "cdn.css" >}}" integrity="{{< param "cdn.css_hash" >}}" crossorigin="anonymous">

    <title>مرحبًا بالعالم!</title>
  </head>
  <body>
    <h1>مرحبًا بالعالم!</h1>

    <!-- Optional JavaScript; choose one of the two! -->

    <!-- Option 1: Bootstrap Bundle with Popper -->
    <script src="{{< param "cdn.js_bundle" >}}" integrity="{{< param "cdn.js_bundle_hash" >}}" crossorigin="anonymous"></script>

    <!-- Option 2: Separate Popper and Bootstrap JS -->
    <!--
    <script src="{{< param "cdn.popper" >}}" integrity="{{< param "cdn.popper_hash" >}}" crossorigin="anonymous"></script>
    <script src="{{< param "cdn.js" >}}" integrity="{{< param "cdn.js_hash" >}}" crossorigin="anonymous"></script>
    -->
  </body>
</html>
```

### RTL examples

Get started with one of our several [RTL examples]({{< docsref "/examples/#rtl" >}}).

## Approach

Our approach to building RTL support into Bootstrap comes with two important decisions that impact how we write and use our CSS:

1. **First, we decided to build it with the [PostCSS RTLCSS](https://github.com/elchininet/postcss-rtlcss) project.** This gives us some powerful features for managing changes and overrides when moving from LTR to RTL. It also allows us to build a directiopn agnostic version of Bootstrap from one codebase.

2. **Second, we've renamed a handful of directional classes to adopt a logical properties approach.** Most of you have already interacted with logical properties thanks to our flex utilities—they replace direction properties like `left` and `right` in favor `start` and `end`. That makes the class names and values appropriate for LTR and RTL without any overhead.

  For example, instead of `.ml-3` for `margin-left`, use `.ms-3`.

Working with RTL, through our source Sass or compiled CSS, shouldn't be much different from our default LTR though.

## Customize from source

When it comes to [customization]({{< docsref "/customize/sass" >}}), the preferred way is to take advantage of variables, maps, and mixins. This approach works the same for RTL, even if it's post-processed from the compiled files, thanks to `PostCSS RTLCSS` works.

### Custom RTL values

Using [PostCSS RTLCSS value directives](https://github.com/elchininet/postcss-rtlcss#value-directives), you can make a variable output a different value for RTL. For example, to decrease the weight for `$font-weight-bold` throughout the codebase, you may use the `/*rtl: {value}*/` syntax:

```scss
$font-weight-bold: 700 #{/* rtl:600 */} !default;
```

Which would output to the following for our default CSS:

```css
[dir="ltr"] dt {
  font-weight: 700;
}

[dir="rtl"] dt {
  font-weight: 600;
}
```

### Alternative font stack

In the case you're using a custom font, be aware that not all fonts support the non-Latin alphabet. To switch from Pan-European to Arabic family, you may need to use `/*rtl:insert: {value}*/` in your font stack to modify the names of font families.

For example, to switch from `Helvetica Neue` font for LTR to `Helvetica Neue Arabic` for RTL, your Sass code could look like this:

```scss
$font-family-sans-serif:
  Helvetica Neue #{"/* rtl:insert:Arabic */"},
  // Cross-platform generic font family (default user interface font)
  system-ui,
  // Safari for macOS and iOS (San Francisco)
  -apple-system,
  // Chrome < 56 for macOS (San Francisco)
  BlinkMacSystemFont,
  // Windows
  "Segoe UI",
  // Android
  Roboto,
  // Basic web fallback
  Arial,
  // Linux
  "Noto Sans",
  // Sans serif fallback
  sans-serif,
  // Emoji fonts
  "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol", "Noto Color Emoji" !default;
```

{{< callout warning >}}
**Edge cases and known limitations** to consider when working with a combined LTR and RTL implementation:

1. When switching `ltr` and `rtl`, make sure you add `dir` and `lang` attributes accordingly.
{{< /callout >}}

## The breadcrumb case

The [breadcrumb separator]({{< docsref "/components/breadcrumb" >}}/#changing-the-separator) is the only case requiring its own brand-new variable— namely `$breadcrumb-divider-flipped` —defaulting to `$breadcrumb-divider`.

## Additional resources

- [PostCSS RTLCSS](https://github.com/elchininet/postcss-rtlcss)
- [RTL Styling 101](https://rtlstyling.com/posts/rtl-styling)
