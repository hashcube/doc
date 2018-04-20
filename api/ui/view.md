# Class: ui.View

Inherits from
:    1. [event.Emitter](./event.html#class-event.emitter)

A view is the base display object for rendering regions to
the screen. Each view has a size and position, and can be nested
within other views to create a scene graph. This scene graph
is a tree of views that composes the visible elements of the
screen, organizing the game content in to a hierarchy which
can be used to define boundaries for capturing input
events. The game engine uses `View` objects to accelerate
performance on mobile devices.

Views have methods for adding and removing superviews and
subviews (parents and children), can set handler functions
for events, and contain properties for styling how the view
is displayed on the screen.

## Examples

* [A Basic View](../example/views-basic/)
* [Changing the View Style](../example/views-style/)
* [Nested Views](../example/views-nested/)
* [Add and Remove Views](../example/views-addremove/)
* [Input Click Event](../example/events-input-click/)
* [Input Move Event](../example/events-input-move/)
* [Input Out Event](../example/events-input-out/)
* [Input Drag Event](../example/events-input-drag/)
* [Modifying a View's Style](../example/views-style/)
* [Rotate a View](../example/views-rotate/)

## Methods

### new View ([options])

Parameters
:    1. `options {object}` ---Optional.
     * `superview {View}` ---Parent or Super view to place this view.
     * `id {string}` ---Unique identifier.
     * `tag {string}` ---Human readable tag for the UI Inspector.
     * `filters {Filter}` ---Filter object.
     * `infinite {boolean} = false` ---Infinite width and height generally for maps or backgrounds.
     * `canHandleEvents {boolean} = true` ---Input events can pass through if `false`.

The constructor used to create an instance of a `ui.View`
object. In addition to the options listed here,
[style definition properties](#properties) can also be
passed in this object.

~~~
import ui.View as View;

var view = new View({
  id: 'MyCrazyView',
  superview: parent,
  x: 50,
  y: 50,
  width: 100,
  height: 100,
  backgroundColor: '#0000ff'
});
~~~

### updateOpts ([options])

Parameters
:    1. `options {object}` ---The options object is the same as defined for the constructor.

Returns
:    1. `{object}` ---Returns the options object.

Update the properties and styles of a view.

### getApp ()

Returns
:    1. `{ui.Engine}`

Returns the root application for the view, [GC.app.engine](./appengine.html#singleton-gc.app.engine).
This is the top-level node of the scene graph, a [ui.Engine](./appengine.html#class-ui.engine) singleton automatically instantiated by the game engine.

### getSuperview ()

Returns
:    1. `{View}` ---Return the view's parent in the scene graph hierarchy.

### getParents ()

Returns
:    1. `{array}` ---A collection of `View` elements.

Returns an array of all parent ancestors of the current view
to the root of the scene graph.

### getSubviews ()

Returns
:    1. `{array}` ---A collection of `View` elements.

Returns an array containing a reference to all of the view's
children. Since this function has an execution time of *O(n)*,
you should store a reference to the array and then iterate:

~~~
for (var i = 0, children = view.getSubviews(), len = children.length; i < len; i++) {
  children[i].style.update({
    x = i * 10,
    y = i * 10
  });
}
~~~

### getSubview (i)

Parameters
:    1. `i {number}` ---Array index position.

Returns
:    1. `{View}` ---Return a child subview at the given array index.

### addSubview (view)

Parameters
:    1. `view {View}` ---The view to add as a child of this view.

Returns
:    2. `{View}` ---Returns the view that was passed to this method.

Add a view as a child subview.

### removeSubview (view)

Parameters
:    1. `view {View}`

Removes a child subview from this view.

### removeAllSubviews ()

Removes all child subviews from this view.

### removeFromSuperview ()

Removes this view from its parent superview.

### needsRepaint ()

Notifies the renderer that the view needs to be redrawn on
next animation frame. This function is only needed if you're
using a DOM rendering backend.

### needsReflow ()

Notifies the `ui.Engine` that the view needs its position updated.

### setHandleEvents (handleEvents [, ignoreSubviews])

Parameters
:    1. `handleEvents {boolean} = true` ---Configure the view to handle or not handle input events.
     2. `ignoreSubviews {boolean} = false` ---Optionally block input events on all subviews.

A view that can not handle events will pass them through to
other views positioned beneath them on the screen. By
default, a view handles all input events. In the *UI Inspector*
you can see which views are underneath the input
cursor by hovering over an element and using *control-click*.

If the `ignoreSubviews` option is set to `true`, all events
on the view's children are also ignored.

### isInputOver ()

Returns
:    1. `{boolean}`

If an input event is over a view, return `true`, otherwise `false`.

### startDrag ([options])

Parameters
:    1. `options {object}`
         * `inputStartEvent {InputEvent} = 'START'`
         * `radius {number} = 0`

Respond to an input event by dragging the view. This will
fire drag events so you can make a view follow the drag events.

The right time to call `startDrag` is inside the `"InputStart"` event:

~~~
view.on("InputStart", function (evt) {
      view.startDrag({
        inputStartEvt: evt,
        radius: 10
      });
});
~~~

In this example, the drag events will start firing after a 10 pixel drag occurs.

To listen for a drag on a view and update its
position:

~~~
view.on('DragStart', function (dragEvt) {
  view.style.x = dragEvt.srcPt.x;
  view.style.y = dragEvt.srcPt.y;
});

view.on('Drag', function (startEvt, dragEvt, delta) {
  view.style.x = dragEvt.srcPt.x;
  view.style.y = dragEvt.srcPt.y;
});

view.on('DragStop', function (startEvt, dragEvt) {
  view.style.x = dragEvt.srcPt.x;
  view.style.y = dragEvt.srcPt.y;
});
~~~

See the DragStart, Drag, and DragStop documentation for more information about handling these events.

### isDragging ()

Returns
:    1. `{boolean}`

Test if the view is being dragged.

### localizePoint (point)

Parameters
:    1. `point {Point}`

Returns
:    1. `{Point}` ---Returns the given point, with updated values.

Convert a point to a local position relative to this view.

### getPosition ([relativeTo])

Parameters
:    1. `relativeTo {View}` ---Optional.

Returns
:    1. `{object}`
         * `x {number}`
         * `y {number}`
         * `rotation {number}`
         * `width {number}`
         * `height {number}`
         * `scale {number}`

Get position of a view relative to a superview. If
`relativeTo` is not provided, get the position relative to
the top-most superview (the root of the scene graph.).

### containsLocalPoint (point)

Parameters
:    1. `point {Point}` ---A point being an object with `x` and `y` numeric properties.

Returns
:    1. `{boolean}`

Determine if the given point is contained by the view.

### getBoundingShape ()

Returns
:    1. `{Rect}`, the shape defined when the view was created.

Return the bounding rectangle for a view.

### getRelativeRegion (region, parent)

Parameters
:    1. `region {Rect}`
     2. `parent {View}`

Returns
:    1. Return `{Rect}`

Return the location of a rectangle region in a parent's coordinate space.

### getFilters ()

Returns
:    1. `{array}`

Return an array of filters attached to a view.

### setFilter (filter)

Parameters
:    1. `filter {Filter}`

Sets the filter on this view. If this view already has a filter, the old filter is removed. A filter can be applied to multiple views, but no view can have more than one filter.

### removeFilter ()

Remove the filter from this view.

### getTag ()

Returns
:    1. `{string}`

Return the human-readable name for a view.

### hide ()

Make the view invisible.

### show ()

Make the view visible.

### focus ()

Returns
:    1. `{this}`

Indicate to the focus manager this view has focus.

### blur ()

Returns
:    1. `{this}`

Indicate to the focus manager this element has been blurred.


## Properties

### style `{object}`

The properties of a view's `style` object determine the look
and style of a view. If not passed as options to a view
constructor, the style properties can be set on this object.

~~~
var view = new View({visible: true}); //now you see me

view.style.visible = false;           //now you don't
~~~

The `style` object contains the following properties:

* `layout {string} = false` ---Options are `'box'` or `'linear'`. For details, see the [Designing User Interfaces Guide](../guide/designing-user-interfaces.html).
* `x {number} = 0` ---The position of the top-left corner of a view on the x-axis relative to its parent.
* `y {number} = 0` ---The position of the top-left corner of a view on the y-axis relative to its parent.
* `offsetX {number} = 0` ---The x position of the anchor point for translation, relative to the top-left corner of the view.
* `offsetY {number} = 0` ---The y position of the anchor point for translation, relative to the top-left corner of the view.
* `anchorX {number} = 0` ---The x position of the anchor point for rotation and scaling, relative to the top-left corner of the view.
* `anchorY {number} = 0` ---The y position of the anchor point for rotation and scaling, relative to the top-left corner of the view.
* `flipX {boolean} = false` ---If `true`, flips the view in-place on the x-axis.
* `flipY {boolean} = false` ---If `true`, flips the view in-place on the y-axis.
* `width {number}` ---The width of a view, defaults to the width of the view's parent.
* `height {number}` ---The height of a view, defaults to the height of the view's parent.
* `widthPercentage {number}`
* `heightPercentage {number}`
* `scale {number} = 1` ---Increase or decrease the size of the view.
* `scaleX {number} = 1` ---Increase or decrease the size of the view in the X direction.
* `scaleY {number} = 1` ---Increase or decrease the size of the view in the Y direction.
* `r {number} = 0` ---The rotation of a view in radians.
* `visible {boolean} = true` ---If the view is shown or hidden. The view will not accept input events while hidden.
* `opacity {number} = 1` ---The transparency of a view from 0.0 to 1.0. The view accepts input events even when fully transparent.
* `zIndex {number} = 0` ---The larger the value, the closer to the front the view appears. This is relative to a view's siblings, not the entire scene graph.
* `backgroundColor {string}` ---Background color of the view.
* `clip {boolean} = false` ---If `true`, child views get clipped to the boundaries of this view.

The following `style` properties are available to any view with a `'box'` or `'linear'` layout:

* `centerAnchor {boolean} = false` ---If `true`, centers anchorX and anchorY, recentering whenever dimensions change.
* `centerX {boolean} = false` ---If `true`, centers x, recentering whenever dimensions change.
* `centerY {boolean} = false` ---If `true`, centers y, recentering whenever dimensions change.

The `style` object contains the following methods:

#### style.update (style)

Parameters
:    1. `style {object}` ---Using the properties enumerated here.

Returns
:    1. `{this}` ---Returns this view.

Update the view's style.

~~~
view.style.update({
  x: 100,
  y: 200,
  backgroundColor: 'red'
});
~~~

#### style.copy ()

Returns
:    1. `{object}`

Returns a shallow copy of the view's style object. You can
use this object to update another view's style.

~~~
var old_style = view1.style.copy();

view2.style.update(old_style);
~~~


## Events
  
There are several input events which can be subscribed to 
on a view.  Subscribing to these events allows you to do 
things such as click or drag a view, or react to these 
events within their respectives callbacks from the event
handler.

### init ()

If you are deriving from View, the object constructor is triggered when a new instance of the view is constructed.

Example of deriving from View in MyView.js :

~~~
exports = Class(View, function(supr) {
	this.init = function(opts) {
		supr(this, 'init', [opts]);

		// Handle class instantiation here
	}

	this.tick = function(dt) {
		// Handle 'dt' milliseconds time passing here
	}
...
~~~

### render (ctx)

By overriding the render call, you are able to bypass the normal rendering of the view to access OpenGL calls on native platforms more directly.  When the view would normally be rendered to the backbuffer, this function is called and you are responsible for providing render commands.

Please see the [Context2D class documentation](./context.html) for more information.

Example of deriving from View in MyView.js :

~~~
exports = Class(View, function(supr) {
	this.init = function(opts) {
		supr(this, 'init', [opts]);

		// Handle class instantiation here
	}

	this.render = function(ctx) {
		// Handle manual rendering operations here
	}
...
~~~

### onFocus ()

The callback function is triggered when focus is given to this view.

~~~
view.onFocus = function () {
  console.log("View has focus!");
};
~~~

### onBlur ()

The callback function is triggered when this view loses focus.

~~~
view.onBlur = function () {
  console.log("View has lost focus!");
};
~~~

### tick (dt)

Parameters
:    1. `dt {number}`

This callback function is executed on every tick of the game engine.

~~~
view.tick = function (dt) {
  this.exampleMethod(dt);
};
~~~

This is a convenience method that can be used instead of
subscribing to the `'Tick'` event on the main game engine:

~~~
GC.app.engine.on('Tick', function (dt) {
  view.exampleMethod(dt);
});
~~~


## Input Events

### \'InputStart\', callback (event, point)

Parameters
:    1. `event {InputEvent}`
     2. `point {Point}`

Fired on a mousedown / touch occurrence. `event` represents
the InputEvent which occurred from InputStart
occurring. `point` is a point relative to the top-left
corner of the view.

Subscribe to the capture-phase event with `'InputStartCapture'`.  

~~~
view.on('InputStart', function (event, point) {
  console.log("This view had touch begin on it at: " + point.x + "," + point.y);
});
~~~

### \'InputSelect\', callback (event, point)

Parameters
:    1. `event {InputEvent}`
     2. `point {Point}`

Fired on a mouseup / touchend occurence. `event` represents
the InputEvent which occurred from InputSelect
occurring. `point` is a point relative to the top-left
corner of the view. The capture-phase event is available by
subscribing to `'InputSelectCapture'`.

~~~
view.on('InputSelect', function (event, point) {
  console.log("View clicked at position: " + point.x + "," + point.y);
});
~~~
 
### \'InputMove\', callback (event, point)

Parameters
:    1. `event {InputEvent}`
     2. `point {Point}`

Fired after an `'InputStart'` event, when the input is moving on the view.  

~~~
view.on('InputMove', function (event, point) {
  console.log("This view had touch begin on it at: " + point.x + "," + point.y);
});
~~~

### \'InputOver\', callback (over, overCount, atTarget)

Parameters
:    1. `over`
     2. `overCount {number}`
     3. `atTarget`

The event is fired when input is moved over a view.
  
~~~
view.on('InputOver', function (over, overCount, atTarget) {
  ...
});
~~~

### \'InputOut\', callback (over, overCount, atTarget)

Parameters
:    1. `over`
     2. `overCount {number}`
     3. `atTarget`

The event is fired when input is moved off a view.  

~~~
view.on('InputOut', function (over, overCount, atTarget) {
  ...
});
~~~

### \'DragStart\', callback (dragEvent)

Parameters
:    1. `dragEvt {InputEvent}`

Fired when dragging starts. `dragEvt` represents the event from which dragging started.  `dragEvt.srcPt` contains the point where the drag started.

~~~
view.on('DragStart', function (dragEvt) {
  console.log("Drag started at (", dragEvt.srcPt.x, ",", dragEvt.srcPt.y, ") screen coordinates" );
});
~~~

### \'Drag\', callback (startEvt, dragEvt, delta)

Parameters
:    1. `startEvt {InputEvent}`
     2. `dragEvt {InputEvent}`
     3. `delta {number}`

Fired during dragging. `startEvt` represents the event from
which dragging started. `dragEvt` represents the event
occuring from movement on the view. `delta` represents the
difference between the last `dragEvt` and this one.

~~~
view.on('Drag', function (startEvt, dragEvt, delta) {
  console.log("Drag continued at (", dragEvt.srcPt.x, ",", dragEvt.srcPt.y, ") screen coordinates" );
});
~~~

### \'DragStop\', callback (dragEvt, selectEvt)

Parameters:
    1. `startEvt {InputEvent}`
    2. `dragEvt {InputEvent}`

Fired when dragging is stopped.  `startEvt` represents the
event from which dragging started.  `dragEvt` represents
the event which occurs when the dragging has stopped.

~~~
view.on('DragStop', function (dragEvent, selectEvent) {
  console.log("Drag ended at (", dragEvt.srcPt.x, ",", dragEvt.srcPt.y, ") screen coordinates" );
});
~~~
