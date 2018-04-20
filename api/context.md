# Class: Context2D

Context2D is mainly exposed through the View::render(ctx) override.
To get an instance of a Context2D object, create a Class that derives
from View, and then define `this.render = function(ctx) {}`.

Context2D supports a subset of the full [W3 HTML5 Canvas 2D Context specification](http://www.w3.org/html/wg/drafts/2dcontext/html5_canvas/).  Please refer to the full documentation where this summary falls short.


## Methods

### clear ()

Clears the context for the view to opaque black.

### swap ()

Flushes images.

### save ()

Pushes current context state on the stack.

### restore ()

Pops current context state off the stack.

### clipRect (x, y, w, h)

Parameters
:    1. `x {number}` ---Upper-left position of clipping rectangle.
:    2. `y {number}` ---Upper-left position of clipping rectangle.
:    3. `w {number}` ---Bounds of clipping rectangle.
:    4. `h {number}` ---Bounds of clipping rectangle.

Clips rendering outside the given region.

### drawImage (img, sx, sy, sw, sh, dx, dy, dw, dh)

Parameters
:    1. `img {Image}` ---Image for source of draw.
:    2. `sx {number}` ---Upper-left position of image draw source.
:    3. `sy {number}` ---Upper-left position of image draw source.
:    4. `sw {number}` ---Bounds of draw rectangle from source image.
:    5. `sh {number}` ---Bounds of draw rectangle from source image.
:    6. `dx {number}` ---Upper-left position of draw on destination view, 0 = left edge of view.
:    7. `dy {number}` ---Upper-left position of draw on destination view, 0 = top edge of view.
:    8. `dw {number}` ---Bounds of draw rectangle on destination view.
:    9. `dh {number}` ---Bounds of draw rectangle on destination view.

Draw a rectangular portion of an image scaled into a destination rectangle on the view.  Composited using the `globalCompositeOperation` property (see below).

### translate (x, y)

Parameters
:    1. `x {number}` ---Offset in X direction.  Positive = right.
:    2. `y {number}` ---Offset in Y direction.  Positive = down.

Translate in the current coordinate system set up by previous Context2D calls.

### rotate (r)

Parameters
:    1. `r {number}` ---A number in radians for rotation.  Positive rotation is counter-clockwise with the screen.

Rotate in the current coordinate system set up by previous Context2D calls.

### scale (x, y)

Parameters
:    1. `x {number}` ---Scale coefficient for X axis.  1.0 = no scaling.
:    2. `y {number}` ---Scale coefficient for Y axis.  1.0 = no scaling.

Scale in the current coordinate system set up by previous Context2D calls.

### setFilters (filters)

Parameters
:    1. `filters {Array}` ---An array of Filter objects.

This sets color filter(s), to modify texture colors in real time.  See the [Filter documentation](./ui-filter.html) for the format of the objects in the array.

### clearFilters ()

Clear all active color filters.

### clearRect (x, y, width, height)

Parameters
:    1. `x {number}` ---Rectangle upper-left corner, relative to view.
:    2. `y {number}` ---Rectangle upper-left corner, relative to view.
:    3. `width {number}` ---Rectangle dimensions.
:    4. `height {number}` ---Rectangle dimensions.

Clear rectangular region to black.

### fillRect (x, y, width, height)

Parameters
:    1. `x {number}` ---Rectangle upper-left corner, relative to view.
:    2. `y {number}` ---Rectangle upper-left corner, relative to view.
:    3. `width {number}` ---Rectangle dimensions.
:    4. `height {number}` ---Rectangle dimensions.

Fill rectangular region with color specified by `fillStyle` property (see below).  Composited using the `globalCompositeOperation` property (see below).

### strokeRect (x, y, width, height)

Parameters
:    1. `x {number}` ---Rectangle upper-left corner, relative to view.
:    2. `y {number}` ---Rectangle upper-left corner, relative to view.
:    3. `width {number}` ---Rectangle dimensions.
:    4. `height {number}` ---Rectangle dimensions.

Render a rectangular outline with color specified by `strokeStyle` property and line width specified by `lineWidth` property.  Composited using the `globalCompositeOperation` property.  See below for more information about these properties.

### fillText (text, x, y, maxWidth)

This method works the same way as the standard HTML5 canvas.  See [W3 documentation for fillText](http://www.w3.org/html/wg/drafts/2dcontext/html5_canvas/#dom-context-2d-filltext).

### strokeText (text, x, y, maxWidth)

This method works the same way as the standard HTML5 canvas.  See [W3 documentation for strokeText](http://www.w3.org/html/wg/drafts/2dcontext/html5_canvas/#dom-context-2d-stroketext).

### measureText (text)

This method works the same way as the standard HTML5 canvas.  See [W3 documentation for measureText](http://www.w3.org/html/wg/drafts/2dcontext/html5_canvas/#dom-context-2d-measuretext).


## Properties

### globalAlpha `{number}`

Set alpha blending coefficient [0 ... 1.0] for all subsequent context render operations.  It is not recommended to use this option, instead setting the alpha per-view is much faster.

### globalCompositeOperation `{string}`

Set the composite operation from one of the following supported types:

* `source-atop`: Display the source image wherever both images are opaque. Display the destination image wherever the destination image is opaque but the source image is transparent. Display transparency elsewhere.
* `source-in`: Display the source image wherever both the source image and destination image are opaque. Display transparency elsewhere.
* `source-out`: Display the source image wherever the source image is opaque and the destination image is transparent. Display transparency elsewhere.
* `source-over`: Display the source image wherever the source image is opaque. Display the destination image elsewhere.
* `destination-atop`: Same as source-atop but using the destination image instead of the source image and vice versa.
* `destination-in`: Same as source-in but using the destination image instead of the source image and vice versa.
* `destination-out`: Same as source-out but using the destination image instead of the source image and vice versa.
* `destination-over`: Same as source-over but using the destination image instead of the source image and vice versa.
* `lighter`: Display the sum of the source image and destination image, with color values approaching 255 (100%) as a limit.
* `xor`: Exclusive OR of the source image and destination image.
* `copy`: Display the source image instead of the destination image.

Used by `drawImage`, `fillRect`, `strokeRect`, `fill`, `stroke`, `fillText`, and `strokeText`.

For more information see the [W3 HTML5 Canvas 2D Context Compositing specification](http://www.w3.org/html/wg/drafts/2dcontext/html5_canvas/#compositing).

### lineWidth `{number}`

Number of pixels in width of the next line-art drawing.

### fillStyle `{string}`

This is one of the HTML standard color string formats:

* "blue" : Standard CSS color names.  See [this website](http://www.w3schools.com/cssref/css_colornames.asp) for a complete list.
* "#330033" : Standard HTML Hexadecimal RGB code.  See [this website](http://www.w3schools.com/cssref/css_colors.asp) for a detailed description of this format.
* "rgb(255,0,0)" : Standard HTML RGB code.  See [this website](http://www.w3schools.com/cssref/css_colors.asp) for a detailed description of this format.
* "rgb(255,0,0,0.8)" : Standard HTML RGB code with alpha value between 0 and 1 in the final parameter.

### strokeStyle `{string}`

This is also one of the HTML standard color string formats (see above).
