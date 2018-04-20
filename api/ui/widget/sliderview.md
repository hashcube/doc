# Class: ui.widget.SliderView

Inherits from
:    1. [ui.ImageScaleView](./ui-images.html#class-ui.imagescaleview)
     2. [ui.View](./ui-view.html)
     3. [event.Emitter](./event.html#class-event.emitter)

The slider is an input element to select a value. The
orientation of the slider can be horizontal or vertical. You
may force the slider to have an orientation of `horizontal`
or `vertical` by setting the `horizontal` property  to
`true` or `false`. If you do not specify this option it will
be `horizontal` if the `width` is greater than the  `height`
and `vertical` if the `height` is `greater` than the width.

<div class="figure-wrapper">
	<figure>
		<img src="./assets/ui-widget-sliderview/slider.png"/>
		<figcaption>A slider, with a green `track` and a red `thumb`.</figcaption>
	</figure>
</div>

There are two states: active and inactive, each state has
associated style properties. When the view is not active
then the slider does not respond to input events.

When the `track` option contains the `activeColor` property
then a color is applied to the background else the
background is styled based on image properties (which should
be passed as and option).

The format of the `track` and `thumb` options to apply
background images is the same as the `ImageScaleView`
options which means you can use 3-slice, 6-slice or 9-slice
images for the background.

## Examples

* [SliderView](../example/ui-slider/)

## Methods

### new SliderView ([options])

Parameters
:    1. `options {object}`
       * `active {boolean} = true` ---Optional, if not passed then the view is active.
       * `horizontal {boolean}` ---Optional, if not passed then the direction is based on the relation between the width and the height.
       * `minValue {number} = 0` ---The minimum value.
       * `maxValue {number} = 100` ---The maximum value.
       * `thumbSize {number} = "auto"` ---The size of the thumb, if `auto` then it will be based on the size of the view.
       * `active {boolean} = true` ---Is the view active?
       * `increment {number} = false` ---If increment is a value then an integer diviseble by the increment will be returned.
       * `track {Object}` ---Information about the styling of the track, equal to `ImageScaleView` options.
		 * `active {string|Image}` ---The active background image or an URL to an active background image.
		 * `inactive {string|Image}` ---The inactive background image or an URL to an inactive background image.
		 * `activeColor {string}` ---The color of the track.
		 * `inactiveColor {string}` ---The color of the track if the view is not active.
       * `thumb {Object}` ---Information about the styling of the thumb, equalt to `ImageScaleView` options.
		 ----
		 * `active {string|Image}` ---The active background image or an URL to an active background image.
		 * `pressed {string|Image}` ---The pressed background image or an URL to an pressed background image.
		 * `inactive {string|Image}` ---The inactive background image or an URL to an inactive background image.
		 * `activeColor {string}` ---The color of the track.
		 * `pressedColor {string}` ---The color of the track when pressed.
		 * `inactiveColor {string}` ---The color of the track if the view is not active.

The constructor used to create an instance of a `ui.widget.SliderView`
object. In addition to the options listed here,
[style definition properties](./ui-view.html#style-object) can also be
passed in this object.

~~~
import ui.widget.SliderView as SliderView;

var sliderView = new SliderView({
  superview: parent,
  x: 50,
  y: 50,
  width: 100,
  height: 30,
  track: {
    activeColor: '#008800',
    inactiveColor: '#E0E0E0'
  },
  thumb: {
    activeColor: '#00AA00',
    pressedColor: '#990000',
    inactiveColor: '#008800'
  }
});
~~~

### updateOpts ([options])

Parameters
:    1. `options {object}` ---The options object is the same as defined for the constructor.

Returns
:    1. `{object}` ---Returns the options object.

Update the properties and styles of a view.

### getValue ()

Returns
:    1. `{number}` ---Return the value

Get the value, if the increment is `false` then the value
can be a floating point value else  it's an integer
diviseble by the increment.

### setIncrement (increment)

Parameters
:    1. `increment {number}`

Set the increment, if the increment is `false` then no increment value is applied.

### setMinValue (minValue)

Parameters
:    1. `minValue {number}`

Set the minimum value, this value should be lower than the maximum value.

### setMaxValue (maxValue)

Parameters
:    1. `maxValue {number}`

Set the maximum value, this value should be higher than the minimum value.

### setThumbSize (thumbSize)

Parameters
:    1. `thumbSize {number}`

Set the thumb size in pixels. If the value is set to `false`
-which is the default- then the size will be determined
based on the size of the view: when the direction is
horizontal then the thumb size is set to the height minus
the  vertical padding, when the direction is horizontal then
the size is set to the width minus the horizontal padding.

Setting the thumb size only affects the size in the slide
direction, if the direction is horizontal then the height
will be determined as described above, when the direction is
vertical then the width will be determined as described
above.

### setValue (value)

Parameters
:    1. `value {number}`

Set the value, the value is clamped on the set minimum and maximum value.

## Events

### \'Change\', callback (value)

Parameters
:    1. `value {Number}`

Fired when the slider is dragged, the minimum or maximum
value changed or the increment changed. When the increment
is set then the value is an integer diviseble by the
increment.

~~~
view.on('Change', function (evt, point) {
  console.log("This view had touch begin on it at: " + point.x + "," + point.y);
});
~~~