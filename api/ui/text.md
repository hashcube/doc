# Class: ui.TextView

Inherits from
:    1. [ui.View](./ui-view.html)
     2. [event.Emitter](./event.html#class-event.emitter)

Display text on a screen within a `View`. The `TextView` class offers different options to handle the size of the
text related to the size of the view. Setting `autoSize` lets the view increase in size when the text does not
fit, setting `autoFontSize` scales the text until it fits in the view.

## Minimum and maximum size

The minimum and maximum size can be controlled with the `style.minWidth`, `style.minHeight`, `style.maxWidth`
and `style.maxHeight` properties. If both `autoSize` and `autoFontSize` are set to true and the view is smaller
than the given maximum size then the view will grow to fit the text (if necessary) and if the maximum size
is reached and the text still does not fit then the font will be scaled.

## Examples

* [TextViews Options](../example/text-textviews/)
* [Lots of TextViews](../example/text-textviews-random/)
* [TextView Clock](../example/text-textviews-clock/)
* [TTF Fonts](../example/text-fonts-ttf)
* [TextPromptView](../example/text-prompt/)
* [TextEditView](../example/text-input/)
* [ScoreView](../example/text-scoreview/)
* [Internationalization](../example/text-internationalization/)

## Methods

### new TextView ([options])

Parameters
:    1. `options {object}` ---`TextView` specific options, also accepts [`View` options](./ui-view.html#new-view-options).
     * `text {string}` ---The text to display.
     * `size {number} = 128` ---The default font size of the text -- scales down to fit view when autoFontSize is true (default).
     * `lineHeight {number} = 1.2` ---How tall each line should be when wrap is turned on.
     * `fontFamily {string} = device.defaultFontFamily` ---Font family to be used by the text in the text view (ex: "Helvetica")
     * `fontWeight {string} = 'normal'` ---How thick the characters are. Options: `'normal'`, `'bold'`, or a number weight.
     * `color {string} = "blue"` ---The color of the text.  See below for color string format.
     * `backgroundColor {string} = "blue"` ---The background color of the text view.  See below for color string format.
     * `strokeColor {string} = undefined` ---The color of the stroke.  See below for color string format.
     * `shadowColor {string} = undefined` ---Color of the drop shadow behind the text.  See below for color string format.
     * `strokeWidth {number} = 2` ---Width of text stroke.
     * `shadowWidth {number} = 2` ---Width of drop shadow.
     * `padding {number|array} = 0` ---The amount of vertical padding the text exhibits within the text view.
     * `verticalAlign: "middle"` ---How the text should be aligned vertically within the text view. Options: `'top'`, `'bottom'`, `'middle'`.
     * `horizontalAlign: "center"` ---How the text should be aligned horizontally within the text view. Options: `'left'`, `'right'`, `'center'`, `'justify'`.
     * `wrap {boolean} = false` ---Whether or not the text should wrap. A description of the rules of wrapping when wrap is turned on can be found below.
     * `hardWrap {boolean}` ---Optional, if true and the text is a single line which is too wide then the string will be split into lines which fit within the given width of the view.
     * `autoSize {boolean} = false` ---Fit the text view to text (details below).
     * `autoFontSize {boolean} = true` ---Fit text to the text view (details below).
     * `buffer {boolean} = true` ---Buffer this `TextView` instead of rerendering every tick.

:    2. Colors (`color`, `backgroundColor`, `shadowColor`, `strokeColor`) can be any of the following values:
     * `null` or `undefined` : No color (transparent).
     * `blue` : Standard CSS color names.  See [this website](http://www.w3schools.com/cssref/css_colornames.asp) for a complete list.
     * `#330033` : Standard HTML Hexadecimal RGB code.  See [this website](http://www.w3schools.com/cssref/css_colors.asp) for a detailed description of this format.
     * `rgb(255,0,0)` : Standard HTML RGB code.  See [this website](http://www.w3schools.com/cssref/css_colors.asp) for a detailed description of this format.
     * `rgba(255,0,0,0.8)` : Standard HTML RGBA code with alpha value between 0 and 1 in the final parameter.

Buffering greatly improves `TextView` performance, but an unlimited amount of text can't fit in the buffer. Therefore, if you have many `TextView`s in your game, it may make sense to set `buffer` to `false` in some cases -- typically, in scenes without many `TextView`s, where performance isn't an issue.

For fonts to work in the emulator, they must be installed system-wide.  For fonts to work on native Android or iOS, be sure to fill out the manifest.json file.  There are some additional considerations for Android and iOS.  Please see the [manifest.json documentation](../guide/manifest.html).

~~~
import ui.TextView as TextView;

var text = new TextView({
  superview: parent,
  x: 25,
  y: 100,
  width: 400,
  text: "Hello, world!",
  size: 24,
  shadowColor: '#999999'
});
~~~

## Sizing

The `TextView` supports several options for handling how a
text is sized within the view. You can control these options
through the properties `'autoSize'`, `'autoFontSize'` or `'wrap'`.

* `autoFontSize = true`, `wrap = true`, `autoSize = true`:
  Wrap words until the text fits horizontally or the line is
  reduced to a single word. If the widest word does not
  fit, decrease the font size. If the resulting lines exceed
  the height of the widget, then increase the height of the widget.
* `autoFontSize = true`, `wrap = false`, `autoSize = true`:
  Measure the maximum line width and the height of the
  lines, then increase the size of the widget if necessary.
* `autoFontSize = true`, `wrap = true`, `autoSize = false`: Wrap
  words until the text fits horizontally or the line is
  reduced to a single word. If the largest word doesn’t fit,
  then reduce the font size. If the resulting number of
  lines exceeds the widget height then reduce the font size.
* `autoFontSize = true`, `wrap = false`, `autoSize = false`:
  Measure the maximum line width and the height of the lines
  and decrease the size of the font is necessary.
* `autoFontSize = false`, `wrap = true`, `autoSize = true`: Wrap
  words until the text fits horizontally or the line is
  reduced to a single word. If the most wide word does not
  fit then increase the width of the widget. If the
  resulting number of lines exceeds the height of the widget
  the increase the height of the widget.
* `autoFontSize = false`, `wrap = false`, `autoSize = true`:
  Measure the maximum line width and the height of the lines
  and increase the size of the widget if necessary.
* `autoFontSize = false`, `wrap = true`, `autoSize = false`:
  Wrap words until the text fits horizontally or the line is
  reduced to a single word. If the text does not fit then is
  will either overflow or be clipped depending on the clip
  setting of the widget. Whether the overflow is top,
  bottom, left or right depends on the alignment settings.
* `autoFontSize = false`, `wrap = false`, `autoSize = false`:
  If the text does not fit then is will either overflow or
  be clipped depending on the clip setting of the
  widget. Whether the overflow is top, bottom, left or right
  depends on the alignment settings.

### setText (text)

Parameters
:    1. `text {string}`

Set the display text. The text view will readjust itself
with the current options using the given text.

### getText ()

Returns
:    1. `{string}`

Return the display text.

### updateOpts (options)

Parameters
:    1. `options {object}`

Update text options.


# Class: ui.TextPromptView

Inherits from
:    1. [ui.TextView](./ui-text.html#class-ui.textview)
     2. [ui.View](./ui-view.html)
	 3. [event.Emitter](./event.html#class-event.emitter)

This class presents the user a prompt for entering text
input. The `TextPromptView` consists of two elements, the
view part which is visible and a dialog. When the view is
clicked a prompt dialog is shown. If the user enters or
changes text and presses ok then the text of the view will
be updated.

## Examples

* [Using a Text Prompt](../example/text-prompt/)

## Methods

### new TextPromptView ([options])

Parameters
:    1. `options {object}`
     * `prompt {text} = ''` ---The text message shown on the dialog.
     * `autoShowKeyboard {boolean} = false` ---Show the keyboard when the dailog is shown on native.
     * `isPassword {boolean} = false` ---This is a password field -- hide input.

~~~
import ui.TextPromptView as TextPromptView;

var textprompt = new TextPromptView({
  prompt: "Please enter your name"
});
~~~

### setPrompt (prompt)

Parameters
:    1. `prompt {string}` ---Set the text to display in the prompt dialog.

### showPrompt ()

Present the input dialog to the user.


## Events

### \'Change\', callback (value)

Parameters
:    1. `value {string}` ---The updated value of the `TextPromptView`.

The `Change` event is published when the text in the dialog
is changed and ok is pressed.

### \'Cancel\', callback ()

The `Cancel` event is published when the cancel button in
the dialog is pressed.


# Class ui.TextEditView

Inherits from
:    1. [ui.ImageScaleView](./ui-images.html#class-ui.imagescaleview)
     2. [ui.View](./ui-view.html)
     3. [event.Emitter](./event.html#class-event.emitter)

This class provides a text input field. Note that currently the `TextEditView` falls back to the `TextPromptView` on iOS and in the simulator.

## Examples

* [Getting Text Input](../example/text-input/)

## Methods

### new TextEditView ([options])

Parameters
:    1. `options {object}`
     * `hint {text} = ''` ---The message shown in the text field.
     * `color {string} = 'black'` ---Color of input text.
     * `hintColor {string} = '#979797'` ---Color of hint text.
     * `paddingLeft {number}` ---Padding around embedded TextView.
     * `paddingRight {number}` ---Padding around embedded TextView.
     * `paddingUp {number}` ---Padding around embedded TextView.
     * `paddingDown {number}` ---Padding around embedded TextView.

~~~
import ui.TextEditView as TextEditView;

var namefield = new TextEditView({
  hint: 'Please enter your name'
});
~~~

### getText ()

Returns contents of text input field.

### setText (text)

Sets input field value to `text`. If `text` is null, sets input field value to hint text.

### registerTextFilter (fn)

Registers `fn` with the `TextEditView`. `fn` is a transformation function that takes one string argument and returns a string. This can be used to process text entered into the input field.


## Events

### \'Change\', callback (value)

Parameters
:    1. `value {string}` ---The updated value of the `TextEditView`.

The `Change` event is published when the text in the input field is changed.


# Class: ui.ScoreView

Inherits from
:    1. [ui.View](./ui-view.html)
     2. [event.Emitter](./event.html#class-event.emitter)

This class is designed for high performance text rendering using images.
It is ideal for scores or other in-game counters that update often.

## Examples

* [Using a ScoreView](../example/text-scoreview/)

## Methods

### new ScoreView ([options])

Parameters
:    1. `options {object}`
     * `characterData {object}` ---The characters to use in this ScoreView. If undefined, you must call setCharacterData before rendering text.
     * `horizontalAlign {string} = 'center'` ---Text alignment - 'center', 'right', or 'left'.
     * `verticalAlign {string} = 'middle'` ---Text alignment - 'middle', 'top', or 'bottom'.
     * `spacing {number} = 0` ---How much space to put between each character.

~~~
import ui.ScoreView as ScoreView;
var yellowScore = new ScoreView({
  characterData: {
    "0": { "image": "resources/images/numbers/yellow_0.png" },
    "1": { "image": "resources/images/numbers/yellow_1.png" },
    "2": { "image": "resources/images/numbers/yellow_2.png" },
    "3": { "image": "resources/images/numbers/yellow_3.png" },
    "4": { "image": "resources/images/numbers/yellow_4.png" },
    "5": { "image": "resources/images/numbers/yellow_5.png" },
    "6": { "image": "resources/images/numbers/yellow_6.png" },
    "7": { "image": "resources/images/numbers/yellow_7.png" },
    "8": { "image": "resources/images/numbers/yellow_8.png" },
    "9": { "image": "resources/images/numbers/yellow_9.png" }
  }
});
~~~

### setCharacterData (data)

Parameters
:    1. `data {object}` ---The character data to use with this SpriteView. Each character object has the following properties:
     * `image {string}` ---The image resource of this character (required).
     * `width {number}` ---If undefined, falls back to actual character width.
     * `offset {number} = 0` ---Optional value for kerning.

### setText (text)

Parameters
:    1. `text {string}` ---The text to display in this ScoreView.

<!--
# Class: ui.TextInputView

Inherits from
:    1. [ui.View](./ui-view.html)
	 2. [event.Emitter](./event.html#class-event.emitter)

Enter text input inline. *This is not working.*

## Methods

### new TextInputView ([options])

Parameters
:    1. `options {object`

~~~
import ui.TextInputView as TextInputView;
~~~
-->


# Class: ui.resource.Font

Basic font representation class.

## Methods

### new Font (font)

Parameters
:    1. `font {string}`

Creates a font from a standard CSS font string.

### new Font ([options])

Parameters
:    1. `options {object}`
	     * `name {string} = device.defaultFamilyName`
		 * `size {number} = 20`
		 * `unit {string} = 'px'`
		 * `style {string}`
		 * `weight {string}`

Creates a font from options.

~~~
import ui.resource.Font as Font;

var font = new Font("16px Helvetica");
~~~

### getName ()

Returns
:    1. `{string}`

Returns the font family.

### getSize ()

Returns
:    1. `{number}`

Returns the font size in pixels.

### getWeight ()

Returns
:    1. `{string}` ----Returns the font weight.

## Class Methods

### Font.parse (font)

Parameters
:    1. `font {string}`

Returns
:    1. `{Font}`

Creates a Font from a font string.
