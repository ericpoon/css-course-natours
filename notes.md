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
