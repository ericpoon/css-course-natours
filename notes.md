##Lecture 14, Section 3

a CSS rule contains a **selector** and a **declaration block**,
in a declaration block there are **declarations**,
and each declaration has **property** and **declared value**

####CSS parsing includes 2 steps:
  1) resolve conflicting CSS declaration
  2) process final CSS values

####Step 1 is also referred as the cascade:
  *Process of combining different stylesheets and resolving conflicts* in 3 sources
  - author declarations (written by developers)
  - user declarations (due to user interaction/behaviour)
  - user agent declarations (default browser declarations)


  Priority: importance > specificity > source order
  - The cascade starts by giving each conflicting declaration an *importance*
    importance level:
      - user !important declarations
      - author !important declarations
      - auth declarations
      - user declarations
      - user agent declarations
  - If same importance, then it compares *specificity*
    specificity level (according to the selector):
      - inline styles
      - IDs
      - classes, pseudo-classes, attributes
      - elements, pseudo-elements
    Note: see 9:01 in lecture 14 for how it counts specificity
  - If same specificity, then it compares *source order*
    use the last declaration to override all previous declarations

  Note:
    - Only use !important as a last resource,
      it's always better to use correct specificities for more maintainable code
    - Universal selector * has no specificity value, it's (0, 0, 0, 0)
    - Rely more on specificity
    - When using 3rd party stylesheets, always put the author stylesheets last,
      so what the developer writes won't be overridden


##Lecture 16, Section 3

###Step 2 of parsing CSS - process final CSS values

Note that all units (%, vh, vw, etc.) are ultimately converted into px in this step

How CSS values are processed:
  There are 6 types of values, during the process:
  1) Declared value
    - author declarations
  2) Cascaded value
    - after cascade (resolving conflicts)
  3) Specified value
    - giving a default/initial value, if there is not cascaded value
    - inheritance plays a role here
  4) Computed value
    - converting relative values to absolute (converting units into px)
  5) Used value
    - final calculations, based on layout
    - this happens later in rendering phase
  6) Actual value
    - due to browser and device restrictions, floating point may be rounded

How units are converted from relative to absolute (px)
  There are two types of percentage:
  1) percentage for fonts:    x% * parent's computed *font-size*
  2) percentage for lengths:  x% * parent's computed *width*

  There are also three types of *font-based units*:
  1) em for fonts:            x * parent's computed font-size
  2) em for lengths:          x * current element's computed font-size
  3) rem:                     x * root's computed font-size
     (calculation way of rem is the same for fonts and lengths)
  - font-based units give us flexibility, we only need to change font size to change all elements' sizes,
    this is a great technique for responsive layout

  Two more viewport-based units
  1) vw:                      x% of the viewport width
  1) vh:                      x% of the viewport height

####Summary
  - each property has an initial value, used if nothing is declared and no inheritance
  - browsers specify a root font-size for each page (usually 16px)
  - % and relative values are always converted to px
    - % - relative to parent's font-size, for font-size
    - % - relative to parent's width, for lengths
    - em - relative to parent's font-size, for font-size
    - em - relative to current font-size, for lengths
    - rem - relative to root's font-size, for both font-size and lengths
    - vw, vh - percentage measurements of viewport's view and height


##Lecture 17, Section 3

###Inheritance in CSS

*Recall that every CSS property must have a value*

if there's a cascaded value --> specified value = cascaded value
else if the property is inherited --> specified value = *computed value* of parent element
else specified value = initial value
  - Note that not all properties can be inherited, see docs
  - See 2:11 in lecture 17 for the flow chart

####Summary
  1) Properties related to text are inherited: font-family, font-size, color, etc.
    - it makes no sense to inherit properties like margin and padding
  2) The computed value of a property is what gets inherited, not the declared value
  3) Inheritance of a property only works if no one declares a value for that property
    - neither the developer nor the browser
  4) The 'inherit' keyword forces inheritance on a certain property
  5) The 'initial' keyword resets a property to its initial value


##Lecture 19, Section 3

###Website rendering: the visual formatting model

Definition: algorithm that calculate boxes and determines the layout of these boxes 
for each element in the render tree in order to determine the final layout of the page

Things to consider when applying visual formatting model algorithm
  - Dimensions of boxes: the box model
  - Box type: inline, block (flex, list-item, table) and inline-block
  - Positioning scheme: floats and positioning (relative, absolute)
  - stacking contexts (?)
  - Other elements in the render tree (parents, siblings, children)
  - Viewport size, dimensions of images, etc.

####Dimensions of boxes: The box model
A box model has:
  - Content: text, image, etc.
  - Padding: transparent area around the content, inside of the box
  - Border: goes around the padding of the content
  - Margin: space between boxes
  - Fill area: area that gets filled with **background color** or **background image** (this includes content, padding and border, but not margin)

Use `box-sizing: border-box` to make width and height include padding and border - make life easier

####Box types: Inline, block and inline-block
1) Block-level boxes
- 100% of parent's width
- vertically, one after another (has line break after each)
- sub-types are *flex*, *list-item* and *table*

2) Inline boxes
- distributed in lines
- occupies only content's space
- no line breaks
- no height and width
- only left and right padding and margin

3) Inline-block boxes
- occupies only content's space
- no line breaks
- has height and width
- has top/bottom padding/margin

####Positioning schemes: Normal flow, absolute positioning and floats
1) Normal flow
- default positioning scheme
- not floated
- not absolutely positioned
- elements laid out according to the HTML element order
```
/* default */
position: relative
```

2) Floats
- element is removed from the normal flow
- text and inline elements will wrap around the floated element
- the container will not adjust its height to the element
```
float: left
float: right
```

3) Absolute positioning
- element is removed from the normal flow
- no impact on surrounding content or elements
- we use `top`, `bottom`, `left` and `right` to offset the element from its first relative positioned container
```
position: absolute
position: fixed
```

####Stacking contexts
With multiple layers (mostly created by z-index), elements can overlap each other

Common misconception: only z-index creates new stacking contexts
- An opacity value different from 1, a transform, a filter or other properties will also create new context

**QUESTIONS** from this lecture:
1) Is `position: relative` the default value for all elements' position?
2) What's Floats? See docs.
3) Differences between `fixed` and `absolute`?
4) What's filter property in CSS?


##Lecture 20, Section 3

Think -> Build -> Architect

###Think: Component-driven design
- Components are re-usable across a project, and between different projects
- Components are independent, allowing us to use them anywhere
- See also Atomic Design - components are analog to organisms

###Build: Block Element Modifier (BEM)
- Block: standalone, meaningful on its own
- Element: part of a block, has no standalone meaning
- Modifier: a different version of a block or an element

Note: naming convention is `.block__element--modifier {}`

####Benefits
1) never nested - always use a single class name
2) very low-specificity selectors
3) easy to maintain

###Architect: 7-1 Pattern
7 different folders for partial Sass files, and 1 main Sass file to import all other files into a compiled CSS stylesheet
1) base/
2) components/
3) layout/
4) pages/
5) themes/
6) abstracts/
7) vendors/


##Lecture 25, Section 4

Mixins, extensions and functions in Sass

###Mixins
Example:
```scss
@mixin style-link-text($color) {
  text-transform: uppercase;
  text-decoration: none;
  color: $color;
}

li {
  a:link {
    @include style-link-text(grey); 
  }
}
```

###Extensions
Example:
- SCSS before compiling
```scss
.icon { // if icon is never used, can use placeholder (%icon) to simplify CSS file
  transition: background-color ease .2s;
  margin: 0 .5em;
}

.error-icon {
  @extend .icon; // or @icon, for placeholder
  /* error specific styles... */
}

.info-icon {
  @extend .icon; // or @icon, for placeholder
  /* info specific styles... */
}
```
- After compiling to CSS
```css
.icon, .error-icon, .info-icon {
  transition: background-color ease .2s;
  margin: 0 .5em;
}

.error-icon {
  /* error specific styles... */
}

.info-icon {
  /* info specific styles... */
}
```

####Comparison between **mixins** and **extensions**
From rendering perspective, mostly they have the same result. 
However, **extensions** modify/copy the selector the achieve this, 
while **mixins** insert/copy the defined properties into the declaration block.
In short, **@extend** copies modifiers while **@include** copies declared properties.

Here's the result of the above example using a mixin (an incorrect usage):
```scss
@mixin icon {
  transition: background-color ease .2s;
  margin: 0 .5em;
}

.error-icon {
  @include icon;
  /* error specific styles... */
}

.info-icon {
  @include icon;
  /* info specific styles... */
}
```
- After compiling to CSS
```css
.error-icon {
  transition: background-color ease .2s;
  margin: 0 .5em;
  /* error specific styles... */
}

.info-icon {
  transition: background-color ease .2s;
  margin: 0 .5em;
  /* info specific styles... */
}
```
Note that this affects the performance of CSS 

More information [here](http://thesassway.com/intermediate/understanding-placeholder-selectors)

###Functions
Sass built-in functions are powerful
```scss
// Sass functions
div {
  color: lighten(red, 10%);
  background-color: darken(blue, 20%);
}
// self-defined function (not very useful)
@function devide($a, $b) {
  @return $a / $b; // note that the returned value has no unit
}
div {
  margin: devide(60, 2) * 1px;
}
```


##Lecture 33, Section 5

###Basic responsive design principles
1) Fluid grids and layouts
    - To allow content to easily adapt to the current viewport width used to browse the website.
    Use % rather than px for all layout-related lengths.
2) Flexible/responsive images
    - Images behave differently than text content, and so we need to ensure that they
    also adapt nicely to current viewport
3) Media queries
    - To change styles on certain viewport widths (breakpoints), allowing us to create
    different version of our website for different widths.

###Layout Typesgs
1) Float layouts
2) Flexbox
3) CSS Grid

Flexbox and CSS grid are more modern, but not supported by all browsers,
the instructor of the course still use float layouts in *production* as of 2017.

Flexbox is great for one-dimensional direction (column or row),
while CSS grid is great for two-dimensional grid.

Note: In this course, float layout will be used in 1st project Natours, and then 2nd one will use flexbox and 3rd one will use CSS grid.
And this is how CSS is moving right now, from float layout to flexbox and to CSS grid.


##Lecture 55, Section 6

###Responsive design: Mobile-first and desktop-first

- Desktop-first: start writing CSS for desktop: large screen; then media queries shrink design to smaller screens
- Mobile-first: start writing CSS for mobile devices: small screen; then media queries expend design to larger screens 

Philosophy behind mobile-first design: it forces us to reduce websites to absolute essentials

**pros of mobile-first:**
- 100% optimized for mobile experience
- reduces websites to the absolute essentials
- results in smaller, faster and efficient products
- prioritizes content over aesthetic design, which may be desirable
**cons of mobile-first:**
- desktop version might feel overly empty and simplistic
- more difficult and counter-intuitive to develop
- less creative freedom, making it more difficult to create distinctive product

Do consider the purpose of the website before making a decision on mobile- or desktop-first design

###Media queries

Media queries don't add any importance or specificity to selectors, so code order matters 

max-width: is width **<=** the specified size (used in desktop-first)
min-width: is width **>=** the specified size (used in mobile-first)

###Breakpoints

- Don't choose breakpoints based on apple's device resolution, this is not general enough and not future-proof
- A good way (usually applied) is to group devices of similar resolution together and set up breakpoints for each group
- The best way is to create breakpoints only when the design does not work any more
(put breakpoints wherever the design starts to look weird, don't think about device at all; but this is very difficult)

Basic breakpoints are for:
1) mobile phone
2) portrait tablet
3) landscape tablet
4) desktop
5) big desktop
