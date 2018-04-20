# Class: ui.StackView

Inherits from
:    1. [ui.View](./ui-view.html)
     2. [event.Emitter](./event.html#class-event.emitter)

A view that contains a collection of child views. Views are
pushed onto the stack and popped off the stack; the view at
the top position of the stack is rendered to the screen.

This is a handy way to organize a collection of game screens, tabs,
or other views that need to be displayed one at a time. The
`StackView` provides a convenient means to navigate through
game screens and be assured the user will not get stuck
without a way out---views can always get popped off the
stack until you get to the root view.

## Examples

* [Basic StackView](../example/ui-stackview-basic/)

## Methods

### new StackView ([options])

Parameters
:    1. `options {object}` ---Options parameters are inherited from the `ui.View` class.

~~~
var stackview = new StackView({
  superview: this,
  x: 0,
  y: 0,
  width: 300,
  height: 300,
  scale: 1,
  zIndex: 3
});
~~~

### getStack ()

Returns
:    1. `{array}` ---Return an array of views contained in this stack.

The bottom view is indexed at 0, and the current view is at
the end of the returned array. Avoid modifying the returned
stack directly, instead use the helper functions below.

### getCurrentView ()

Returns
:    1. `{View}`

Return the current visible view at the top of the stack.

### hasView (view)

Parameters
:    1. `view {View}`

Returns
:    2. `{boolean}` ---Return `true` if the given view is contained in this stack.

### push (view [, noAnimate, reverse])

Parameters
:    1. `view {View}`
	 2. `noAnimate {boolean} = false` ---If `true`, do not animate the view when displaying.
	 3. `reverse {boolean} = false` ---By default, wipe in from the right. If `true`, wipe in from the left.

Returns
:    1. `{View}` ---Same as the provided view object.

Add a view to the `StackView` collection. Since it is now on
top of the stack it will be visible. The `height` and `width`
of the added view will be scaled up to fit the dimensions of
the `StackView`. Additionally, the new view's `y` value is
forced to zero so that the new view perfectly fills the `StackView`.

By default, when a view is pushed to the top of a
`StackView`, it will animate in from right to left. When
`noAnimate` is set to `true`, the view will not animate
in, and will simply appear on top. If `reverse` is set to
`true`, the view will animate in from the left to right.

~~~
// x, y, width, and height are all set by the StackView
var view = new ui.View({});

stackview.push(view);
~~~

### pop ([noAnimate, reverse])

Parameters
:    1. `noAnimate {boolean} = false` ---If `true`, do *not* animate in the new top view.
	 2. `reverse {boolean} = true` ---By default, the view is animated from right to left. If `false`, animate the wipe from left to right.

Returns
:    1. Return: `{View}`

Remove a view from the top of the `StackView`. Unless
`noAnimate` is true, this animates the view to the right,
makes it invisible, then removes it as a subview.

### popAll ([noAnimate])

Parameters
:    1. `noAnimate {boolean} = false` ---If `true`, do *not* animate out the child views.

Removes all child views from the `StackView`, allowing it to
be re-used. Note that this should be used instead of
`removeAllSubviews` as it clears the inner stack.

### remove (view)

Parameters
:    1. `view {View}`

Removes a child view from the stack. Will not animate out
regardless of position.


## Events

#### \'ViewWillAppear\', callback ()

Event emitted to the child view when it is schedules to
appear, but before it has animated in.

#### \'ViewDidAppear\', callback ()

Event emitted to the child view once it has appeared, and
after any animations.

#### \'ViewWillDisappear\', callback ()

Event emitted to the child view when it is scheduled to
disappear, but before it has animated out.

#### \'ViewDidDisappear\', callback ()

Event emitted to the child view once it has disappeared,
after any animations.
