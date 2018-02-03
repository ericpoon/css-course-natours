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