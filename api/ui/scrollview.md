# Class: ui.ScrollView

Inherits from
:    1. [ui.View](./ui-view.html)
     2. [event.Emitter](./event.html#class-event.emitter)

View to scroll itself and subviews. Supports
drag through user input. Useful for implementing
2D cameras, viewports or interfaces that are too
large for a phone screen and require scrolling.

## Examples

[A Basic ScrollView](../example/ui-scrollview-basic/)

## Methods

### new ScrollView ([options])

Parameters
:    1. `options {object}`
	     * `offsetX {number} = 0` ---Offset the child's X position relative to the scrollview.
		 * `offsetY {number} = 0` ---Offset the child's Y position relative to the scrollview.
		 * `scrollX {boolean} = true` ---Allow scrolling along the X axis.
		 * `scrollY {boolean} = true` ---Allow scrolling along the Y axis.
		 * `clip {boolean} = true` ---Hide any subview content that is outside of the view boundary.
		 * `bounce {boolean} = true` ---Bounce effect when scrolling past the scroll bounds.
		 * `bounceRadius {number} = 50` ---Radius of bounce. If bounceRadius is 'bounds', infer bounce radius from scroll bounds.
		 * `drag {boolean} = true` ---Allow the user to drag the view around.
		 * `dragRadius {number} = 10` ---Radius between dragging start and move.
		 * `inertia {boolean} = true` ---Inertia effect when scrolling.
		 * `scrollBounds {object}` ---Boundary of scroll.
		     * `minX {number}` ---Minimum X position.
			 * `minY {number}` ---Minimum Y position.
			 * `maxX {number}` ---Maximum X position.
			 * `maxY {number}` ---Maximum Y position.
		 * `snapPixels {number} = 1` ---Snap to certain pixel increments. This value will represent the grid size to snap to.
		 * `useLayoutBounds {boolean} = false` ---Automatically set scroll bounds when using layout system.

~~~
import ui.ScrollView as ScrollView;

//scroll around an image
var scrollview = new ui.ScrollView({
  superview: parent,
  x: 0,
  y: 0,
  width: 480,    //the scrollview is smaller than the image
  height: 320,
  offsetX: -100, //offset the image within the scrollview
  offsetY: -100, //by 100 pixels
  scrollBounds: {
    minX: 0,
    maxX: 1000,  //can scroll 1000 px to the right
    minY: 0,
    maxY: 1000   //can scroll 1000 px downwards.
  }
});

var worldmap = new ui.ImageView({
  x: 0,
  y: 0,
  width: 1000,
  height: 1000,
  image: "resources/images/worldmap.png"
});

scrollview.addSubview(worldmap); //add the big image to the scrollview
~~~

`ScrollView` will create a subview called `contentView`. This is where all
the subviews will be placed and the `contentView` will be positioned
to imitate scrolling.

### addFixedView (view)

Parameters
:    1. `view {View}` ---The view to add as a fixed child of this view.

Returns
:    1. `{View}` ---Returns the view that was passed to this method.

Add a non-scrolling child subview.

### removeFixedView (view)

Parameters
:    1. `view {View}` ---The fixed child view to remove from this view.

Returns
:    1. `{View}` ---Returns the view that was passed to this method.

Remove a non-scrolling child subview.

### getStyleBounds ()

Returns
:    1. `{object}`
	     * `minX {number}` ---The minimum horizontal boundary.
		 * `maxX {number}` ---The maximum horizontal boundary.
		 * `minY {number}` ---The minimum vertical boundary.
		 * `maxY {number}` ---The maximum vertical boundary.

Return the style boundary object.

### getOffset ()

Returns
:    1. `{Point}`
	     * `x {number}` ---The current horizontal offset.
		 * `y {number}` ---The current vertical offset.

Returns an instance of Point containing the scroll offset.

### setOffset (x, y)

Parameters
:    1. `x {number}` ---The horizontal offset to set.
     2. `y {number}` ---The vertical offset to set.

Set the offset to manually scroll views. Similar to `scrollTo` but
without animation.

### isScrolling ()

Returns
:    1. `{boolean}`

Return `true` if the ScrollView is scrolling.

### stopScrolling ()

Stop `View` movement during its scrolling animation.

### setScrollBounds (bounds)

Parameters
:    1. `bounds {object}`
	     * `minX {number}` ---The minumum horizontal scroll boundary to set.
		 * `maxX {number}` ---The maximum horizontal scroll boundary to set.
		 * `minY {number}` ---The minimum vertical scroll boundary to set.
		 * `maxY {number}` ---The maximum vertical scroll boundary to set.

Set the scroll boundary.

### getScrollBounds ()

Parameters
:    1. `bounds {object}`
	     * `minX {number}` ---The minumum horizontal scroll boundary.
		 * `maxX {number}` ---The maximum horizontal scroll boundary.
		 * `minY {number}` ---The minimum vertical scroll boundary.
		 * `maxY {number}` ---The maximum vertical scroll boundary.

Return the scroll boundary object.

### addOffset (x, y)

Parameters
:    1. `x {number}` ---The horizontal offset to add.
	 2. `y {number}` ---The vertical offset to add.

Add the values to the current offset. Similar to `scrollTo`
but without animation.

### getContentView ()

Returns
:    1. `{View}`

Return the `contentView` that is created to wrap all the `ScrollView` subviews.

### onInputScroll (scrollEvent)

Parameters
:    1. `scrollEvent {ScrollEvent}`
	     * `scrollAxis {number}` ---Enum, `input.VERTICAL_AXIS`, `input.HORIZONTAL_AXIS`.
		 * `scrollDelta {number}` ---The scroll delta.

This is called in the desktop browser upon using the scroll wheel or a multi-touch scroll action.

### scrollTo (x, y, duration, callback)

Parameters
:    1. `x {number}` ---The horizontal offset to scroll to.
	 2. `y {number}` ---The vertical offset to scroll to.
	 3. `duration {number}` ---The millisecond duration of the scroll animation.
	 4. `callback {function}` ---Function to be invoked after animation completes.

Smoothly scroll to a specific position.

## Events

### \'Scrolled\', callback (delta)

Parameters
:    1. `delta {point}`

Published in `setOffset`; called by `onDrag`, `onDragStop`, and `addOffset`.
