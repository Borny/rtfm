# SVG - Scalable Vector Graphics

- [About](#about)
- [Importing SVG in HTML](#importing-svg-in-html)
- [ViewPort and ViewBox](#viewport-and-viewbox)
- [Basic Shapes](#basic-shapes)
  - [Line](#line)
  - [Rectangle](#rectangle)
  - [Circle](#circle)
  - [Ellipse](#ellipse)
  - [Polyline](#polyline)
  - [Polygon](#polygon)
  - [Path](#path)
  - [Text](#text)
  - [Image](#image)
- [Stroke properties](#stroke-properties)
- [Fill properties](#fill-properties)
- [Transformations](#transformations)
- [Patterns](#patterns)
- [defs](#defs)

## About

`SVG` stands for `Scalable Vector Graphics`. It is a markup language for describing two-dimensional graphics applications and images, and a set of related graphics script interfaces.  
SVG images and their behaviors are defined in `XML text` files. This means that they can be searched, indexed, scripted, and compressed.  
As XML files, SVG images can be created and edited with any text editor, but are more often created with drawing software.

## Importing SVG in HTML

They are multiple ways to import SVG in HTML.

- Using the `<img>` tag:

```html
<img src="image.svg" alt="SVG Image">
```

- Using the `background-image` property in CSS:

```css
div {
  background-image: url('image.svg');
}
```

- Directly embedding the SVG code in the HTML file:

```html
<svg width="100" height="100">
  <circle cx="50" cy="50" r="40" stroke="black" stroke-width="3" fill="red" />
</svg>
```

## ViewPort and ViewBox

The `viewBox` attribute is used to define the coordinate system of the SVG. It is a list of four numbers: `min-x`, `min-y`, `width` and `height`.

```html
<svg viewBox="0 0 100 100">
  <circle cx="50" cy="50" r="40" stroke="black" stroke-width="3" fill="red" />

  <!-- The above circle will be drawn in a 100x100 coordinate system -->
</svg>
```

The `viewBox` attribute can also be used to scale the SVG.

```html
<svg viewBox="0 0 100 100" width="200" height="200">
  <circle cx="50" cy="50" r="40" stroke="black" stroke-width="3" fill="red" />

  <!-- The above circle will be drawn in a 100x100 coordinate system and scaled to 200x200 -->
</svg>
```

## Basic Shapes

### Line

x1: x-coordinate of the start of the line
y1: y-coordinate of the start of the line
x2: x-coordinate of the end of the line
y2: y-coordinate of the end of the line

```html
<svg width="100" height="100">
  <line x1="10" y1="10" x2="90" y2="90" stroke="black" stroke-width="3" />
</svg>
```

### Rectangle

x: x-coordinate of the top-left corner of the rectangle
y: y-coordinate of the top-left corner of the rectangle
width: width of the rectangle
height: height of the rectangle
stroke: color of the border
stroke-width: width of the border
fill: color of the rectangle
rx: radius of the corners
ry: radius of the corners

```html
<svg width="100" height="100">
  <rect
    x="10" y="10" width="80" height="80" stroke="black" stroke-width="3" fill="red" />
</svg>
```

### Circle

- cx: x-coordinate of the center of the circle
- cy: y-coordinate of the center of the circle
- r: radius of the circle
- stroke: color of the border
- stroke-width: width of the border
- fill: color of the circle

```html
<svg width="100" height="100">
  <circle cx="50" cy="50" r="40" stroke="black" stroke-width="3" fill="red" />
</svg>
```

### Ellipse

- cx: x-coordinate of the center of the ellipse
- cy: y-coordinate of the center of the ellipse
- rx: horizontal radius of the ellipse
- ry: vertical radius of the ellipse
- stroke: color of the border
- stroke-width: width of the border
- fill: color of the ellipse

```html
<svg width="100" height="100">
  <ellipse cx="50" cy="50" rx="40" ry="20" stroke="black" stroke-width="3" fill="red" />
</svg>
```

### Polyline

Polyline is a series of connected lines. It won't automatically close the shape.

- points: list of points separated by spaces

```html
<svg width="100" height="100">
  <polyline points="10,10 90,10 90,90 10,90 10,10" stroke="black" stroke-width="3" fill="none" />
</svg>
```

### Polygon

Same as the polyline shape, the Polygon is a series of connected lines. But it `will automatically close` the shape.

- points: list of points separated by spaces

```html
<svg width="100" height="100">
  <polygon points="10,10 90,10 90,90 10,90" stroke="black" stroke-width="3" fill="red" />
</svg>
```

### Path

The `path` element is used to define a path.

- d: path data

```html
<svg width="100" height="100">
  <path d="M10 10 L90 10 L90 90 L10 90 Z" stroke="black" stroke-width="3" fill="red" />
</svg>
```

The `d` attribute contains a series of commands and parameters. Each command is a letter followed by numbers. The commands are:

- M: Move to
- L: Line to
- H: Horizontal line to
- V: Vertical line to
- C: Cubic Bezier curve
- S: Smooth Cubic Bezier curve
- Q: Quadratic Bezier curve
- T: Smooth Quadratic Bezier curve
- A: Elliptical Arc
- Z: Close path

```html
<svg width="100" height="100">
  <path d="M10 10 L90 10 L90 90 L10 90 Z" stroke="black" stroke-width="3" fill="red" />
</svg>
```

Cubic Bezier curve

```html
<svg width="100" height="100">
  <path d="M10 10 C90 10 90 90 10 90 Z" stroke="black" stroke-width="3" fill="red" />
</svg>
```

Quadratic Bezier curve

```html
<svg width="100" height="100">
  <path d="M10 10 Q50 90 90 90 Z" stroke="black" stroke-width="3" fill="red" />
</svg>
```

Elliptical Arc

```html
<svg width="100" height="100">
  <path d="M10 10 A40 40 0 0 1 90 90 Z" stroke="black" stroke-width="3" fill="red" />
</svg>
```

## Text

The `text` element is used to define text.

- x: x-coordinate of the text
- y: y-coordinate of the text
- font-family: font family
- font-size: font size
- fill: color of the text

```html
<svg width="100" height="100">
  <text x="10" y="50" font-family="Arial" font-size="16" fill="black">Hello <tspan x="30" y="50">World</tspan></text>
</svg>
```

## Image

The `image` element is used to define an image.

- x: x-coordinate of the top-left corner of the image
- y: y-coordinate of the top-left corner of the image
- width: width of the image
- height: height of the image
- xlink:href: URL of the image

```html
<svg width="100" height="100">
  <image x="10" y="10" width="80" height="80" xlink:href="image.jpg" />
</svg>
```

## Stroke properties

- stroke: color of the border
- stroke-width: width of the border
- stroke-linecap: shape to be used at the end of open subpaths
- stroke-linejoin: shape to be used at the corners of paths
- stroke-dasharray: pattern of dashes and gaps
- stroke-dashoffset: offset of the dash pattern

```html
<svg width="100" height="100">
  <line x1="10" y1="10" x2="90" y2="90" stroke="black" stroke-width="3" stroke-linecap="round" stroke-linejoin="round" stroke-dasharray="5,5" stroke-dashoffset="2" />
</svg>
```

## Fill properties

- fill: color of the shape
- fill-opacity: opacity of the fill
- fill-rule: rule used to determine the inside of the shape

```html
<svg width="100" height="100">
  <circle cx="50" cy="50" r="40" stroke="black" stroke-width="3" fill="red" fill-opacity="0.5" fill-rule="evenodd" />
</svg>
```

## Transformations

- translate(x, y): moves the element by x and y
- scale(x, y): scales the element by x and y
- rotate(angle, cx, cy): rotates the element by angle around the point cx, cy
- skewX(angle): skews the element by angle along the x-axis
- skewY(angle): skews the element by angle along the y-axis
- matrix(a, b, c, d, e, f): applies a transformation matrix

```html
<svg width="100" height="100">
  <circle cx="50" cy="50" r="40" stroke="black" stroke-width="3" fill="red" transform="translate(10,10) scale(2,2) rotate(45,50,50) skewX(30) skewY(30) matrix(1,0,0,1,0,0)" />
</svg>
```

## Patterns

The `pattern` element is used to define a pattern.

- x: x-coordinate of the top-left corner of the pattern
- y: y-coordinate of the top-left corner of the pattern
- width: width of the pattern
- height: height of the pattern
- patternUnits: coordinate system of the pattern
- patternContentUnits: coordinate system of the content
- patternTransform: transformation of the pattern
- viewBox: coordinate system of the pattern
- preserveAspectRatio: aspect ratio to preserve

```html
<svg width="100" height="100">
  <defs>
    <pattern id="pattern" x="0" y="0" width="20" height="20" patternUnits="userSpaceOnUse">
      <circle cx="10" cy="10" r="5" fill="red" />
    </pattern>
  </defs>
  <rect x="0" y="0" width="100" height="100" fill="url(#pattern)" />
</svg>
```

## defs

The `defs` element is used to define elements that can be reused.

```html
<svg width="100" height="100">
  <defs>
    <circle id="circle" cx="50" cy="50" r="40" stroke="black" stroke-width="3" fill="red" />
  </defs>
  <use href="#circle" />
  <use href="#circle" x="100" />
</svg>
```

## Gradients

### Linear Gradient

The `linearGradient` element is used to define a linear gradient.

- x1: x-coordinate of the start of the gradient
- y1: y-coordinate of the start of the gradient
- x2: x-coordinate of the end of the gradient
- y2: y-coordinate of the end of the gradient
- gradientUnits: coordinate system of the gradient
- gradientTransform: transformation of the gradient
- spreadMethod: method to use when the gradient starts or ends

```html
<svg width="100" height="100">
  <linearGradient id="gradient" x1="0" y1="0" x2="100" y2="100">
    <stop offset="0" stop-color="red" />
    <stop offset="1" stop-color="blue" />
  </linearGradient>

  <rect x="0" y="0" width="100" height="100" fill="url(#gradient)" />
</svg>
```

### Radial Gradient

The `radialGradient` element is used to define a radial gradient.

- cx: x-coordinate of the center of the gradient
- cy: y-coordinate of the center of the gradient
- r: radius of the gradient
- fx: x-coordinate of the focal point of the gradient
- fy: y-coordinate of the focal point of the gradient
- fr: radius of the focal point of the gradient

```html
<svg width="100" height="100">
  <radialGradient id="gradient" cx="50" cy="50" r="50" fx="50" fy="50" fr="0">
    <stop offset="0" stop-color="red" />
    <stop offset="1" stop-color="blue" />
  </radialGradient>

  <rect x="0" y="0" width="100" height="100" fill="url(#gradient)" />
</svg>
```

## clipPath

The `clipPath` element is used to define a clipping path.

- clipPathUnits: coordinate system of the clipping path
- transform: transformation of the clipping path

```html
<svg width="100" height="100">
  <defs>
    <clipPath id="clip">
      <circle cx="50" cy="50" r="40" />
    </clipPath>
  </defs>
  <rect x="0" y="0" width="100" height="100" fill="red" clip-path="url(#clip)" />
</svg>
```

## Filters

The `filter` element is used to define a filter.

- x: x-coordinate of the top-left corner of the filter
- y: y-coordinate of the top-left corner of the filter
- width: width of the filter
- height: height of the filter
- filterUnits: coordinate system of the filter
- primitiveUnits: coordinate system of the primitives
- filterRes: resolution of the filter
- x: x-coordinate of the top-left corner of the filter
- y: y-coordinate of the top-left corner of the filter
- width: width of the filter
- height: height of the filter
- filterUnits: coordinate system of the filter
- primitiveUnits: coordinate system of the primitives
- filterRes: resolution of the filter

```html
<svg width="100" height="100">
  <defs>
    <filter id="filter">
      <feGaussianBlur in="SourceGraphic" stdDeviation="5" />
    </filter>
  </defs>
  <rect x="0" y="0" width="100" height="100" fill="red" filter="url(#filter)" />
</svg>
```