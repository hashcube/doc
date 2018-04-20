# Class: ui.widget.ButtonView

This class provides a quick and easy way to create Buttons or any other 
widget with button-like states (such as checkboxes, switches or radiobuttons).
You can listen for state changes, automatically swap an image or play a sound
for entering a state.

Because this class is a subclass of `ImageScaleView` you can use several options
of applying background images, see [ui.ImageScaleView](./ui-images.html#class-ui.imagescaleview) 
for more details.

## Examples

* [Button: Basic demo](../example/ui-button/)
* [Button: Toggle demo](../example/ui-button-toggle/)

## Title

The `ButtonView` has a title which is a `TextView` child. The settings for the title can be
applied by passing a `text` object in the constructor options. See [ui.TextView](./ui-text.html#class-ui.textview) 
for information about the format of the options.

## Icon

The `ButtonView` has an icon which is an `ImageView` child. The settings for the icon can be
applied by passing an `icon` object in the constructor options. See [ui.ImageScale](./ui-images.html#class-ui.imageview) 
for information about the format of the options.

## Toggle option

The `ButtonView` class can also function as an interface element to select or deselect an option like a checkbox.
When using the `ButtonView` as a checkbox then only the `images.selected` and `images.unselected` have to be
assigned, the `images.up` and `images.down` properties are optional. 

## Sound

You may have sounds play whenever the button enters a state by providing an instance
of [`AudioManager`](./audio.html) in the option `audioManager`. A corresponding
object option named `sounds` is required where the key is the state and the
value is the name of the sound to play (this just calls [`AudioManager#play()`](./audio.html#play-name-options) with
the provided value).

Inherits from
:    1. [ui.ImageScaleView](./ui-images.html#class-ui.imagescaleview)
     2. [ui.View](./ui-view.html)
     3. [event.Emitter](./event.html#class-event.emitter)

## Methods

### new ButtonView([options])

Parameters
:    1. `options {object}`
		* `onClick {function}` ---Shortcut way of assigning a callback to a click.
		* `clickOnce {boolean} = false` ---When true, button will enter `DISABLED` state after a click.
		* `state {ButtonView.states}` ---Optional, the initial state of the button, the default is `ButtonView.states.UP`
		* `toggleSelected {boolean} = false` ---When true, button will toggle in between `SELECTED` and `UNSELECTED` state on every click.
		* `title {string}` ---Optional, The title of the button
		* `text {object}` ---These options are applied to the title text which is an instance of `TextView`
		* `icon {object}` ---These options are applied to the icon image which is an instance of `ImageView`
		* `on {object}` ---Callbacks for each state
			* `up {string}` ---Optional, The callback which will be called when the button changes to the `up` state
			* `down {string}` ---Optional, The callback which will be called when the button changes to the `down` state
			* `disabled {string}` ---Optional, The callback which will be called when the button changes to the `disabled` state
			* `selected {string}` ---Optional, The callback which will be called when the button changes to the `selected` state
			* `unselected {string}` ---Optional, The callback which will be called when the button changes to the `unselected` state
		* `images {object}` ---Change the image to the specified image upon entering a state. The key refers to the state and the value is either `Image` or a `string` image path
			* `up {string}` ---Optional, The filename or instance of `Image` to show on the `up` state
			* `down {string}` ---Optional, The filename or instance of `Image` to show on the `down` state
			* `disabled {string}` ---Optional, The filename or instance of `Image` to show on the `disabled` state
			* `selected {string}` ---Optional, The filename or instance of `Image` to show on the `selected` state
			* `unselected {string}` ---Optional, The filename or instance of `Image` to show on the `unselected` state
		* `audioManager {AudioManager}` ---Optional, an instance of AudioManager that contains the sounds to play. The `sounds` object will play a sound on this instance.
		* `sounds {object}` ---Run a sound when the button enters a state
			* `up {string}` ---Optional, The name of the sound effect to play on up
			* `down {string}` ---Optional, The name of the sound effect to play on down
			* `disabled {string}` ---Optional, The name of the sound effect to play on `disabled`
			* `selected {string}` ---Optional, The name of the sound effect to play on `selected`
			* `unselected {string}` ---Optional, The name of the sound effect to play on `unselected`

Change images and play a sound when the button toggles selected state.

~~~
import ui.widget.ButtonView as ButtonView;

var buttonview = new ButtonView({
  superview: parent,
  width: 200,
  height: 100,
  x: 0,
  y: 0,
  images: {
    down: "resources/images/selected.png",
    up: "resources/images/unselect.png"
  },
  on: {
    down: function () {
      console.log("This function is called when the button transitions to down!");
    },
    up: function () {
      console.log("This function is called when the button transitions to up!");
    }
  }
});
~~~

### updateOpts({options})

Parameters
:    1. `options {object}`

Replaces the current options (including `images`, `sounds`
and `on`) with the `options` object passed to this
function. This replaces, and does not merge, the current
options. This is similar to `TextView.updateOpts`. This
means that you may want to `util.merge` the object you pass
in with the current options, like in the example below.

~~~
var buttonview = new ButtonView({
  superview: parent,
  width: 200,
  height: 100,
  x: 0,
  y: 0,
  sounds: {
    "down": "resources/ting.mp3"
  }
});

//now, to change the "down" sound, we must use updateOpts
//however, we wish to keep the other options, so we shall `merge` the objects

buttonView.updateOpts(util.merge(buttonView._opts, {
  sounds: {
    "down": "resources/bang.mp3"
  }
}));
~~~

### getText ()

Returns
:    1. `{TextView}`

Get the `TextView` instance which contains the title text.

### setTitle (title)

Parameters
:    1. `title {string}`

Set the title, updates the `text` property in the `TextView` child.

### getIcon ()

Returns
:    1. `{ImageView}`

Get the `ImageView` instance which contains the icon image.

### setIcon (icon)

Parameters
:    1. `icon {string|Image}`

Set the icon image, the parameter can be a string with the path to the image or an instance of `Image`.

### setState (state)

Parameters
:    1. `state {ButtonView.state}`

Set the state of the image. When the state is selected the background image will also be updated.

## States

The button can have several states, when the `toggleSelected` option is set to `true` then the state switches
between `ButtonView.states.SELECTED` and `ButtonView.states.UNSELECTED` and vice versa.
The `toggleSelected` option also works in concert with the `ButtonView.states.UP` and `ButtonView.states.DOWN` 
states.

If the `toggleSelected` state is `false` then the images and sounds associated with the 
`ButtonView.states.SELECTED` and `ButtonView.states.UNSELECTED` states -which are `images.selected`, `images.unselected`,
`sounds.selected` and `sounds.unselected`- will never be used.

The states are defined in an enum, `ButtonView.states`:
:    1. `UP`
	 2. `DOWN`
	 3. `DISABLED`
	 4. `SELECTED`
	 5. `UNSELECTED`

## Events

### \'up\', callback ()

This callback is set with the `on.up` option.
Emitted when the button state is `up`.

### \'down\', callback ()

This callback is set with the `on.down` option.
Emitted when the button state is `down`.

### \'disabled\', callback ()

This callback is set with the `on.disabled` option.
Emitted when the button state becomes `disabled`.

### \'selected\', callback ()

This callback is set with the `on.selected` option.
Emitted when the button selected state becomes `selected`.

### \'unselected\', callback ()

This callback is set with the `on.unselected` option.
Emitted when the button selected state becomes `unselected`.
