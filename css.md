# CSS

- [CSS](#css)
  - [Rule](#rule)
  - [Selectors](#selectors)
    - [Specificity](#specificity)
      - [The following list of selectors is by increasing specificity:](#the-following-list-of-selectors-is-by-increasing-specificity)
      - [Specificity hierarchy](#specificity-hierarchy)
      - [Specificity calculation](#specificity-calculation)
    - [Important](#important)
    - [Combinators](#combinators)
  - [Box sizing](#box-sizing)
  - [Pseudo classes](#pseudo-classes)
  - [Pseudo elements](#pseudo-elements)
  - [Background property](#background-property)
    - [Using the shorthand](#using-the-shorthand)

## Rule

A **rule**, or **rule-set**, is a statement that dictates how certain elements on a webpage should be displayed. A CSS rule consists of two main parts: a **selector** and a **declaration block**.

The **selector** is used to select the HTML elements that you want to style.

The **declaration block** contains one or more declarations, each of which consists of a property and a value, separated by a colon. Each declaration is separated by a semicolon. The declaration block is enclosed in curly braces {}.

Example:

```css
p { color: red; text-align: center; } 
/* p is the selector and the curly braces is the declaration block */
/* color is the property and red is the value*/
```

## Selectors

### Specificity

#### The following list of selectors is by increasing specificity:

1. Universal selector (`*`)
2. Type selector (e.g., `h1`)
3. Class selector (e.g., `.example`)
4. Attribute selector (e.g., `[type="radio"]`)
5. Pseudo-class (e.g., `:hover`)
6. ID selector (e.g., `#example`)
7. Inline style (e.g., `style="font-weight: bold;"`)

#### Specificity hierarchy

1. Inline styles
2. IDs
3. Classes, pseudo-classes, attribute
4. Elements and pseudo-elements

#### Specificity calculation

1. Count the number of IDs in the selector.
2. Count the number of classes, attributes, and pseudo-classes in the selector.
3. Count the number of elements and pseudo-elements in the selector.

For example:

```css
/* Specificity: 0, 1, 0 */
.example {}

/* Specificity: 0, 0, 1 */
#example {}

/* Specificity: 0, 1, 1 */
.example#example {}

/* Specificity: 0, 2, 1 */
.example.example {}

/* Specificity: 0, 1, 2 */
.example:hover {}

/* Specificity: 1, 0, 0 */
h1 {}

/* Specificity: 0, 0, 0 */
* {}
```

### Important

The `!important` declaration overrides all other declarations. It should be used sparingly, as it makes debugging more difficult.

```css
.example {
  color: red !important;
}
```

### Combinators
  
<!-- write the documentation for the css combinators -->
Combinators are used in CSS selectors to combine multiple selectors in order to target specific elements.

There are four types of combinators in CSS:

1. Descendant combinator (`space`): Selects all elements that are descendants of a specified element.
   Example: `div p` selects all `<p>` elements that are descendants of `<div>` elements.

2. Child combinator (>): Selects all elements that are direct children of a specified element.
   Example: `div > p` selects all `<p>` elements that are direct children of `<div>` elements.

3. Adjacent sibling combinator (+): Selects the element that is immediately following a specified element.
   Example: `h1 + p` selects the `<p>` element that is immediately following an `<h1>` element.

4. General sibling combinator (~): Selects all elements that are siblings of a specified element.
   Example: `h1 ~ p` selects all `<p>` elements that are siblings of an `<h1>` element.

For more information, refer to the CSS documentation on combinators.

## Box sizing

The `box-sizing` property is used to specify how the total width and height of an element is calculated.

By default the `box-sizing` property is set to `content-box`, which means that the width and height of an element does not include padding, border, or margin.

Most of the time the `box-sizing` property is set to `border-box`, the width and height of an element includes padding and border, but not margin. That rule is applied to all elements in the document.

Example:
```css
* {
  box-sizing: border-box;
}
```

## Pseudo classes

<!-- Write documentation for the pseudo classes -->

Pseudo-classes are used to define the **special state** of an element. They are used to style an element when it is in a certain state, such as when a user **hovers** over an element or when an element is the **first child of its parent**.

Pseudo-classes are preceded by a colon `:` and are written after the selector.

Example:
```css
a:hover { /*This will had a red color when hovering hover a link element*/
  color: red;
}
```

## Pseudo elements

<!-- Write documentation for the pseudo elements -->

Pseudo-elements are used to style a **specific part** of an element. They are used to style the **first letter** or **first line** of a block of text, or to insert **content** before or after an element.

Pseudo-elements are preceded by two colons `::` and are written after the selector.

Example:
```css
p::first-line { /*This will make the first line of a paragraph bold*/
  font-weight: bold;
}
```

## Background property

The `background` property is used to set the background color, image, and other properties of an element.

The `background` property is a shorthand property that can be used to set the following properties:

- `background-color`: Sets the background color of an element.
- `background-image`: Sets the background image of an element.
- `background-repeat`: Sets how the background image is repeated.
- `background-attachment`: Sets whether the background image is fixed or scrolls with the page.
- `background-position`: Sets the position of the background image.
- `background-size`: Sets the size of the background image.
- `background-origin`: Sets where the background image is positioned relative to the border box.
- `background-clip`: Sets whether the background extends underneath the border or padding box.
- `background-blend-mode`: Sets how the background image blends with the background color.
- `background-attachment`: Sets whether the background image is fixed or scrolls with the page.

Example:
```css
.example {
  /* dont use shorthand in the example */
  background-color: red;
  background-image: url('example.jpg');
  background-repeat: no-repeat;
  background-attachment: fixed;
  background-position: center;
  background-size: cover;
  background-origin: content-box;
  background-clip: padding-box;
  background-blend-mode: multiply;
}
```

### Using the shorthand 

Here is how you can use the shorthand property to set the background color, image, and other properties:

```css
.example {
  background: red url('example.jpg') no-repeat fixed center / cover content-box padding-box multiply;
  /* background: color image repeat attachment position / size origin clip blend-mode; */
}
```
