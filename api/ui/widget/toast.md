# Class: ui.widget.Toast

Inherits from
:    1. [ui.ImageView](./ui-images.html#class-ui.imageview)
     2. [ui.View](./ui-view.html)
     3. [event.Emitter](./event.html#class-event.emitter)

Widget for showing toast notifications.

## Examples

* [Toast](../example/ui-toast/)

## Methods

### new Toast ([options])

Parameters
:    1. `options {object}`
	     * `position {string} = 'bottom'` ---Where should this toast pop up?
		 * `images {object} = {}` ---What does your toast look like? Only define what you need.
		 * `debug {boolean} = false` ---If true, your toast gets a red background.

You can make a `Toast` with any of the following values for `position`: 'top', 'topright', 'bottomright', 'bottom', 'bottomleft', 'topleft'. These are also the keys of the `images` object.

~~~
import ui.widget.Toast as Toast;

var toast = new Toast({
	superview: mainView,
	position: 'topright',
	images: {
		topright: 'resources/images/toaster.png'
	}
});
~~~

### setImages (images)

Parameters
:    1. `images {object} = {}` ---Object mapping positions to image urls.

Use this function to specify toast images to use in different positions.

~~~
toast.setImages({
	top: 'resources/images/top.png',
	bottom: 'resources/images/bottom.png',
	topright: 'resources/images/right.png',
	bottomright: 'resources/images/right.png',
	topleft: 'resources/images/left.png',
	bottomleft: 'resources/images/left.png'
});
~~~

### setPosition (position)

Parameters
:    1. `position {string} = undefined` ---Where should this toast pop up?

Use this function to indicate where you want the toast to pop up.

~~~
toast.setPosition('topleft');
~~~

### pop (text, position)

Parameters
:    1. `text {string} = undefined` ---What should this toast say?
     2. `position {string} = undefined` ---If defined, switch to this position.

Use this function to pop your toast onto the screen.

~~~
toast.pop('hello!');
~~~