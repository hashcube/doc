# Class: ui.Color

The `Color` class is provided as a convenience for parsing and
storing color values from a number of different formats.

## Methods

### new Color ([options])
1. `options {object}` ---Optional.
    * `r {number} = 0` ---The red value of the color, from 0 to 255.
    * `g {number} = 0` ---The green value of the color, from 0 to 255.
    * `b {number} = 0` ---The blue value of the color, from 0 to 255.
    * `a {number} = 1` ---The alpha value of the color, from 0 to 1.

Color component values can be passed as object properties to
the constructor:

~~~
import ui.Color as Color;

var rgba = new Color({r: 23, g: 234, b: 0, a: 0.8});
~~~

### new Color ([r, g, b, a])

Parameters
:    1. `r {number} = 0` ---The red value of the color, from 0 to 255.
	 2. `g {number} = 0` ---The green value of the color, from 0 to 255.
	 3. `b {number} = 0` ---The blue value of the color, from 0 to 255.
	 4. `a {number} = 1` ---The alpha value of the color, from 0 to 1.

Component values can also be passed as individual parameters to the constructor:

~~~
var rgba = new Color(23, 234, 0, 0.8);
~~~

### new Color ([rgba])

Parameters
:    1. `rgba {string}` ---A color string in hexadecimal or CSS-style format.

Colors can be one of the following values:

* "blue" : Standard CSS color names.  See [this website](http://www.w3schools.com/cssref/css_colornames.asp) for a complete list.
* "#330033" : Standard HTML Hexadecimal RGB code.  See [this website](http://www.w3schools.com/cssref/css_colors.asp) for a detailed description of this format.
* "rgb(255,0,0)" : Standard HTML RGB code.  See [this website](http://www.w3schools.com/cssref/css_colors.asp) for a detailed description of this format.
* "rgb(255,0,0,0.8)" : Standard HTML RGB code with alpha value between 0 and 1 in the final parameter.

~~~
var rgba = new Color('rgba(23,234,0,0.8)');
~~~

### set (color)

Parameters
:    1. `color {object}`
	     * `r {number} = 0` ---Red component, a number between 0 and 255.
		 * `g {number} = 0` ---Green component, a number between 0 and 255.
		 * `b {number} = 0` ---Blue component, a number between 0 and 255.
		 * `a {number} = 1` ---Optional alpha component, a number between 0 and 1.

Set the component values of a color.

~~~
rgba.set({r: 87, g: 24, b: 220, a: 0.7});
~~~

### get ()

Returns
:    1. `color {object}`
	     * `r {number}` ---Red component, a number between 0 and 255.
		 * `g {number}` ---Green component, a number between 0 and 255.
		 * `b {number}` ---Blue component, a number between 0 and 255.
		 * `a {number}` ---Alpha component, a number between 0 and 1.

Return a new object containing the component values of this color.

### parse (color)

Parameters
:    1. `color {string}` ---Color string in hexadecimal or CSS-style format.

Parse a color string and set the red-green-blue-alpha component values.

~~~
rgba.parse('#ff00ff'); //Hexadecimal format

rgba.parse('rgba(255,0,255,0.5)'); //CSS-style RGBA format
~~~

### toString ()

Returns
:    1. `{string}`

Returns the color as a string in the CSS-style format: `'rgba(R, G, B, A)'`.

### toHex ()

Returns
:    1. `{string}`

Returns the color as a string in the hexadecimal format: `'#RRGGBB'`.


## Properties

### r `{number} = 0`

The red component of the color. A number between 0 and 255.

### g `{number} = 0`

The green component of the color. A number between 0 and 255.

### b `{number} = 0`

The blue component of the color. A number between 0 and 255.

### a `{number} = 1`

The alpha component of the color. A number between 0.0 and 1.0.
