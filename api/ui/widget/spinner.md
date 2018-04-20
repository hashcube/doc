# Class: ui.widget.Spinner

Inherits from
:    1. [ui.View](./ui-view.html)
     2. [event.Emitter](./event.html#class-event.emitter)

Display an animated spinner. Great for showing that the
application is not broke (yet).

## Methods

### new Spinner ([options])

Parameters
:    1. `options {object}`
	     * `cycles {number} = 0.5` ---Optional.
		 * `radius {number} = 10` ---Optional.
		 * `spokes {number} = 20` ---Optional.
		 * `thickness {number} = 2` ---Optional.
		 * `trail {number} = 10` ---Optional.
		 * `color {color} = '#ffffff` ---Optional. Color of the spinner lines.
		 * `backgroundColor {color} = ''rgba(0, 0, 0, 0.5)''` ---Optional. Background color of spinner.

~~~
import ui.widget.Spinner as Spinner;

var spinner = new Spinner({
  color: '#0000ff'
});
~~~
