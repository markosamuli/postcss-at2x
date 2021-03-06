# postcss-at2x [![Build Status](https://travis-ci.org/markosamuli/postcss-at2x.svg)](https://travis-ci.org/markosamuli/postcss-at2x) ![node (scoped)](https://img.shields.io/node/v/@markosamuli/postcss-at2x.svg) ![npm (scoped)](https://img.shields.io/npm/v/@markosamuli/postcss-at2x.svg) ![GitHub last commit](https://img.shields.io/github/last-commit/markosamuli/postcss-at2x.svg) [![Known Vulnerabilities](https://snyk.io/test/npm/@markosamuli/postcss-at2x/badge.svg)](https://snyk.io/test/npm/@markosamuli/postcss-at2x)

Forked from [postcss-at2x](https://github.com/simonsmith/postcss-at2x) to
address security vulnerabilities in the development dependencies.

Moved @babel/cli into devDependencies from dependencies to avoid installing it
in applications that use this package.

Ported from [rework-plugin-at2x](https://github.com/reworkcss/rework-plugin-at2x)

## Installation

```console
npm install @markosamuli/postcss-at2x --save-dev
```

## Usage

```js
const fs = require('fs');
const postcss = require('postcss');
const at2x = require('@markosamuli/postcss-at2x');

const input = fs.readFileSync('input.css', 'utf8');

const output = postcss()
  .use(at2x())
  .process(input)
  .then(result => console.log(result.css));
```

### .at2x()

Adds `at-2x` keyword to `background` and `background-image` declarations to add retina support for images.

**Input**

```css
.multi {
  background: url(http://example.com/image.png),
              linear-gradient(to right, rgba(255, 255, 255, 0),  rgba(255, 255, 255, 1)),
              green,
              url(/public/images/cool.png) at-2x;
}
```

**Output**

```css
.multi {
  background: url(http://example.com/image.png),
              linear-gradient(to right, rgba(255, 255, 255, 0),  rgba(255, 255, 255, 1)),
              green,
              url(/public/images/cool.png);
}

@media (min-device-pixel-ratio: 1.5), (min-resolution: 144dpi), (min-resolution: 1.5dppx) {
  .multi {
    background-image: url(http://example.com/image.png), 
                      linear-gradient(to right, rgba(255, 255, 255, 0),  rgba(255, 255, 255, 1)), 
                      none,
                      url(/public/images/cool@2x.png);
  }
}
```

### Options

##### `identifier` (default: `"@2x"`) _string_

Change the identifier added to retina images, for example `file@2x.png` can be `file-retina.png`.

##### `detectImageSize` (default: `false`) _boolean_

Obtains the image dimensions of the non-retina image automatically and applies them to the
`background-size` property of the retina image.

##### `skipMissingRetina` (default: `false`) _boolean_

If the retina image cannot be found on the file system it will be skipped and
not output into the result CSS.

##### `resolveImagePath` _function_

Get resolved image path for detecting image size. By default, original `url` value is resolved from current working directory (`process.cwd()`).

Function receives two arguments: original `url` value and [PostCSS declaration source](http://api.postcss.org/Declaration.html#source).

**Output**

```css
.element {
  background: url(img.jpg) no-repeat;
}

@media (min-device-pixel-ratio: 1.5), (min-resolution: 144dpi), (min-resolution: 1.5dppx) {
  .element {
    background: url(img@2x.jpg) no-repeat;
    background-size: 540px 675px; /* Dimensions of img.jpg */
  }
}
```

See [PostCSS](https://github.com/postcss/postcss/) docs for examples for your environment.
