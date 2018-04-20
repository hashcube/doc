# Class: ui.GestureView

Inherits from
:    1. [ui.View](./ui-view.html)
     2. [event.Emitter](./event.html#class-event.emitter)

The GestureView emits gesture events: `Swipe`, `Pinch`, `Rotate`, `DragSingle`, `FingerDown`, `FingerUp`, and `ClearMulti`.
This makes it easy to handle all your input in one place, and gives you access to
high-level input events without bloating your game code.

## Examples

[Gesture Input](../example/events-input-gesture/)

## Methods

### new GestureView ([options])

Parameters
:    1. `options {object}`
	     * `swipeMagnitude {number} = 150` ---How far your finger has to travel to generate a `Swipe` event.
	     * `swipeTime {number} = 250` ---How long a swipe can last, start to finish.

~~~
import ui.GestureView as GestureView;

var gestureView = new GestureView({
	superview: parent,
	layout: 'box'
});
~~~


## Events

The GestureView emits events.

### Swipe

The GestureView emits a `Swipe` event whenever a drag exceeds `swipeMagnitude` in less than `swipeTime`.

~~~
gestureView.on('Swipe', function(angle, direction, numberOfFingers) {
	console.log('You swiped', direction, '(' + angle, 'degrees) with', numberOfFingers, 'fingers');
});
~~~

### Pinch

When two fingers are placed on the touch screen, the GestureView remembers their initial distance
from each other. Then, as you drag, it emits `Pinch` events with the new distance relative to the
initial distance (newDistance / initialDistance). So a 1 means the distance hasn't changed, a 0.5
means the current distance is half of the initial distance, and a 2 means the current distance is
twice the initial distance.

~~~
gestureView.on('Pinch', function(d) {
	console.log('New finger distance is', d, 'times initial finger distance');
});
~~~

### Rotate

When two fingers are placed on the touch screen, the GestureView remembers the angle of the line
segment connecting the two points. Then, as you drag, it emits `Rotate` events with the new angle
relative to the initial angle (newAngle - initialAngle). So a 0 means the angle hasn't changed,
and any other value indicates a rotation.

~~~
gestureView.on('Rotate', function(r) {
	console.log('Your fingers have rotated', r, 'radians');
});
~~~

### DragSingle

Every View emits `Drag` events. This works great for single touch applications, but not so well
when more than one finger wants to get involved. Say you have a view that you want to move around
in response to `Drag` events. What happens when two fingers are dragging? Should it move twice as
fast? Should it only respond to `Drag` events from one finger, ignoring others? These are life's
big questions, and we don't expect you to answer them on your own. This is why the GestureView
emits `DragSingle` events, which only fire for the _primary_ finger, the one that's been touching
the screen the longest.

~~~
gestureView.on('DragSingle', function(dx, dy) {
	console.log('The primary finger just dragged with dx', dx, 'and dy', dy);
});
~~~

### FingerDown

The GestureView emits a `FingerDown` event whenever you touch the screen.

~~~
gestureView.on('FingerDown', function(fingerCount) {
	console.log('You touched the screen. Fingers down: ' + fingerCount);
});
~~~

### FingerUp

The GestureView emits a `FingerUp` event whenever you lift a finger.

~~~
gestureView.on('FingerUp', function(fingerCount) {
	console.log('You lifted a finger. Fingers remaining: ' + fingerCount);
});
~~~

### ClearMulti

As we mentioned above, the GestureView keeps track of the initial angle and distance between fingers
involved in `Pinch` and `Rotate` events. Whenever your active finger count falls below two, the
GestureView clears these values and fires a `ClearMulti` event to keep other parts of the project
in sync.

~~~
gestureView.on('ClearMulti', function() {
	console.log('Cleared multitouch values!');
});
~~~