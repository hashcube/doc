# Module: animate

`animate` is a module used for programmatically
moving, changing, and animating properties, especially
[ui.View](../api/ui-view.html) properties. It is a
"tweening engine" that interpolates between screen
positions, opacity, or any other numeric JavaScript
property. It's most important feature is that it's optimized
for mobile devices using native code, so you should favor
this module instead of manual position calculations within
your game loop.

## Examples

* [Basic Animation](../example/animation-basic/)
* [Continuous Animation](../example/animation-continuous/)
* [Pause an Animation](../example/animation-pause/)
* [Create a Trail Animation](../example/animation-trail/)
* [Multiple Animations](../example/animation-multibox/)
* [Particle Engine](../example/animation-particles/)
* [Prebuilt Spritesheets](../example/animation-prebuilt/)

## Methods

### animate (obj, groupID)

Parameters
:    1. `obj {ui.View|object}` ---The view or object to animate.
     2. `groupID {number}` ---The group identifier for this animation, [see below](#class-group)

Returns
:    1. `{Animator}` ---Returns the animator object so the function call can be chained.

Create an Animator for a [View](../api/ui-view.html) or
generic JavaScript object. Note that when animating a
[View](../api/ui-view.html) object, the animation engine
will tween based on the object's `style` property. But when
passing in a generic object, the animation engine will tween
based on the object's top level properties instead.

~~~
import animate;

var animator = animate(myView);

// typical usage is via method chaining
// moves a view 100px to the right, then 100px downwards.
animate(myView).now({x: 100}).then({y: 100});
~~~

### animate.getGroup (id)

Parameters
:    1. `id {number}` ---The group identifier of the group to fetch.

Returns
:    1. `{Group}`

Returns the Group with the specified
identifier. [See below](#class-group) for more information on groups.


# Class: animate.Animator

Inherits from
:    1. [event.Emitter](./event.html#class-event.emitter)

The animation engine.


## Methods

### new Animator (subject, group)

Parameters
:    1. `subject {object}` ---The view or object to animate.
     2. `group {Group}` ---The group object for this animation, [see below](#class-group)

Creates a new Animator with a [View](../api/ui-view.html) or
object to animate, with an optional Group to add them
to. Note: the preferred method for creating an animator
object is not to use the `new` keyword, but instead to
invoke the animate function:

~~~
// do not use:
// var animator = new animate.Animator(object);
// instead use this:
var animator = animate(object);
~~~

### clear ()

Returns
:    1. `{this}`

Clears any animation frames currently scheduled. This will
stop the animation immediately without completing any
additional frames.

~~~
var myAnimation = animate(view).now({x: 100}, 5000);

setTimeout( function() {
  myAnimation.clear(); //view.x has stopped in its tracks, without reaching the target value.
}, 1500);
~~~

### pause ()

Pauses the animation.

### isPaused ()

Returns
:    1. `{boolean}`

Returns whether the animation is paused.

### resume ()

Resumes the animation if paused.

### hasFrames ()

Returns
:    1. `{boolean}`

Returns whether there are any frames left in the animation queue.

### wait (duration)

Parameters
:    1. `duration {number}` ---Duration of the wait in milliseconds.

Returns
:    1. `{this}`

Adds a delay (in milliseconds) to the animation queue. This
function can be invoked instead of `setTimeout`, and when
used in conjunction with `animator.then(callback)` affords
greater timing control than `setTimeout`.

~~~
animate(view).wait(500).then(function () {
  console.log("called after 500ms");
});
~~~

### now (target, duration, transition, onTick)

Parameters
:    1. `target {View|object}` ---Will interpolate the appropriate number values of the provided object.
     2. `duration {number}` ---Duration of the animation in milliseconds.
     3. `transition {number}` ---Type of animation transition. See below for transition types.
     4. `onTick {function}` ---A callback to control the speed of the transition.

Returns
:    1. `{this}`

Starts an animation frame immediately. An animation frame is
defined by the duration, transition type, and properties
that are being "tweened" by the animation engine.

An animation transition can be one of the following:

* `animate.linear` ---Animation has the same speed from start to finish.
* `animate.easeIn` ---Animation has a slow start.
* `animate.easeOut` ---Animation has a slow end.
* `animate.easeInOut` ---Animation has both slow start and slow end.

### then (target, duration, transition, onTick)

Parameters
:    1. `target {View|object}` ---Will interpolate the appropriate number values of the provided object.
     2. `duration {number}` ---Duration of the animation in milliseconds.
     3. `transition {number}` ---Type of animation transition. See above for transition types.
     4. `onTick {function}` ---A callback to control the speed of the transition.

Returns
:    1. Return: `{this}`

Similar to `.now()`, but adds the animation frame to the
object's animation queue rather than animating the frame
immediately.

~~~
animate(view).then({x: 500, y: 500}, 1000);

// these can be chained
animate(view).now({x: 10}, 300).then({
  x: 500,
  y: 500
}, 1000).then({
  x: 50,
  y: 50
}, 200).then({
  x: 100,
  y: 10
}, 1000);
~~~

### then (callback)

Parameters
:    1. `callback {function}` ---The callback function to be invoked at the appropriate point in the queue.

Returns
:    1. `{this}`

Adds a callback function to the queue. Useful for running
code after an animation frame completes.

~~~
animate(view).now({x: 10}).then(function(){
  console.log('view has been moved to x = 10.');
});
~~~

You can also specify a duration after the callback.  This will execute the given function multiple times once per tick until the duration ends.  Note this will execute the given function multiple times and not just once after the duration ends; use `wait` (above) to achieve this.

### debug ()

Returns
:    1. `{this}`

Turns debug logging on. This will log information about the
current animation frame like duration, transition, and what
properties are being animated.

### commit ()

Returns
:    1. `{this}`

Finishes the animation immediately, moving all values to the
position specified by the final frame in the animation
queue.

~~~
var myAnimation = animate(view).now({
  x: 100
}, 5000);

setTimeout( function() {
  myAnimation.commit(); //view.x is now at 100 and the animation is complete.
}, 1500);
~~~


# Class: Group

Inherits from
:    1. [event.Emitter](./event.html#class-event.emitter)

A group of animations. Groups are typically used to track a
complex animation involving multiple view animations.

~~~
animate(firstView, 'complexAnimation1').now({ //the second argument is the group id 
  x: 100
}, 100);

animate(secondView, 'complexAnimation1').now({
  x: 100
}, 200);

animate.getGroup('complexAnimation1').on('Finish', function(){
  console.log('The complex animation is finished!');
});
~~~


## Methods

### get (id)

Parameters
:    1. `id {string}` ---The identifier of the animation to fetch from the group.

Returns
:    1. `{Animator}`

Returns an animation from the group.

~~~
var animGroup = animate.getGroup('complexAnimation1');

var anim = animGroup.get('loopDeLoops');
~~~

### add (id, animation)

Parameters
:    1. `id {string}` ---The identifier of the animation to add to the group.
     2. `animation {Animator}`

Returns
:    1. `{Animator}`

Adds an animation to the group and returns it.

### isActive ()

Returns
:    1. `{boolean}`

Returns true if any animations in the group have frames in
their animation queue.

### onAnimationFinish (animation)

Parameters
:     1. `animation {Animator}` ---The animation object to signal as being finished.

Removes an animation from the group. If none of the
remaining animations in the group have frames left in their
queue, the `finish` event will be published.


## Events

### \'Finish\'

Published when all animations in the group have completed
all the frames in their respective animation queues.


# Class: ParticleEngine

Inherits from
:    1. [ui.View](./ui-view.html)

The goal of this class is to facilitate high performance
view animation with minimal garbage collection.

## Methods

### obtainParticleArray (count)

Parameters
:    1. `count {number}` ---How many particle objects to return.

Returns
:    1. `{Array}` ---Returns an array of `count` particle objects.

Call `obtainParticleArray` to get the array of particle objects from the engine. These objects should be modified and passed into `emitParticles` when they're ready. Note that there is only one particle objects array per engine. This array is provided by the engine, and there's no need to create your own.

### emitParticles (pArray)

Parameters
:    1. `pArray {array}` ---An array of particle objects to animate.

Call `emitParticles` with the array of particle objects to queue them up for animation. Remember that this is the same array returned by `obtainParticleArray`.

### runTick (dt)

Parameters
:    1. `dt {number}` ---How many milliseconds of particle animations to run.

Calling `runTick` runs enqueued particle animations for `dt` milliseconds. This arrangement means that you can have hundreds of particle engines in your game, and none of them will tick unless you explicitly ask them to. It also makes it easy to speed up or slow down your effects by simply scaling the `dt` you pass into `runTick`.

## Overview

`ParticleEngine.js` uses a pool of `ImageView`s to animate special effects as defined by particle objects. To create an effect, first call `obtainParticleArray(n)` where n is the number of particles you want in your effect. You will receive an array with n particle object literals pre-populated with their default property values. Modify the particle object properties to define how they move through space and time. Once all of your objects' properties are established, pass the same array back to the particle engine via `emitParticles(array)`. The arrays, object literals, and `ImageView`s are all managed internally by the engine to minimize garbage creation and collection.

Set up a particle engine like this:

~~~
import ui.ParticleEngine as ParticleEngine;

this.pEngine = new ParticleEngine({
  superview: someView,
  width: 1,
  height: 1,
  initCount: 10
});
~~~

When the time is right, emit particles like this:

~~~
var particleObjects = this.pEngine.obtainParticleArray(10);
for (var i = 0; i < 10; i++) {
  var pObj = particleObjects[i];
  pObj.dx = Math.random() * 100;
  pObj.dy = Math.random() * 100;
  pObj.width = 20;
  pObj.height = 20;
  pObj.image = 'resources/images/sparkle.png';
}
this.pEngine.emitParticles(particleObjects);
~~~

Don't forget to update the particle engine in your tick function:

~~~
var tick = function(dt) {
  // .. tick logic ..
  this.pEngine.runTick(dt);
  // .. more tick logic ..
};
~~~

These properties control the size and opacity of the particle over time. All deltas are in units per second.

### Size

* `width {number}`       width (default: 1)
* `dwidth {number}`      delta width (default: 0)
* `ddwidth {number}`     delta delta width (default: 0)
* `height {number}`      height (default: 1)
* `dheight {number}`     delta height (default: 0)
* `ddheight {number}`    delta delta height (default: 0)
* `scale {number}`       scale (default: 1)
* `dscale {number}`      delta scale (default: 0)
* `ddscale {number}`     delta delta scale (default: 0)

### Opacity

* `opacity {number}`     opacity (default: 1)
* `dopacity {number}`    delta opacity (default: 0)
* `ddopacity {number}`   delta delta opacity (default: 0)

All numeric particle properties can have their own velocity and acceleration set by prefixing 'd' and 'dd', respectively, to the property name. For example, if you want to stretch a particle's width, and then have it shrink, you might define particle properties like this:

~~~
pObj.width = 100;
pObj.dwidth = 50;
pObj.ddwidth = -75;
~~~

The following properties govern the basic timing and appearance of the particle.

### Lifespan

* `ttl {number}`         time to live in milliseconds (default: 1000)
* `delay {number}`       time in ms before particle goes active (default: 0)

### Other

* `image {string}`        the image URL used for this particle (default: null)
* `transition {string}`   transition function ID (default: "linear")
* `onStart {function}`    called when a particle becomes active (default: null)
* `onDeath {function}`    called when a particle finishes (default: null)

`transition` can be one of: "linear", "easeIn", "easeInOut", "easeOut".

There are two ways to define the motion of particle data objects:

* Cartesian Physics
* Polar Physics


## Cartesian Physics

If you like properties like `x` and `y`, you'll love Cartesian Physics. Here are the basic properties you'll need:

### Position

* `x {number}`           starting x position (default: 0)
* `y {number}`           starting y position (default: 0)
* `r {number}`           rotation (default: 0)
* `anchorX {number}`     x anchor (default: 0)
* `anchorY {number}`     y anchor (default: 0)

And here are the corresponding delta properties:

### Velocity

* `dx {number}`          delta x (default: 0)
* `dy {number}`          delta y (default: 0)
* `dr {number}`          delta r (default: 0)
* `danchorX {number}`    delta anchor x (default: 0)
* `danchorY {number}`    delta anchor y (default: 0)

### Acceleration

* `ddx {number}`         delta delta x (default: 0)
* `ddy {number}`         delta delta y (default: 0)
* `ddr {number}`         delta delta r (default: 0)
* `ddanchorX {number}`   delta delta anchor x (default: 0)
* `ddanchorY {number}`   delta delta anchor y (default: 0)

Say I wanted to launch of bunch of particles up and to the right and let gravity take over. Say I also wanted a bit of variance. I'd probably do something like this:

~~~
var particleObjects = this.pEngine.obtainParticleArray(10);
for (var i = 0; i < 10; i++) {
  var pObj = particleObjects[i];
  pObj.dx = 100 + Math.random() * 20;
  pObj.dy = -100 + Math.random() * 20;
  pObj.ddy = 50; // gravity is same for all particles
  pObj.width = 20;
  pObj.height = 20;
  pObj.image = 'resources/images/sparkle.png';
}
this.pEngine.emitParticles(particleObjects);
~~~

This will shoot a light spray of particles up and to the right, to be rained down in due time thanks to delta delta y.


## Polar Physics

Enjoy circles, angles, and vectors? Then Polar Physics is for you.

* `polar {boolean}`      set true to use polar coordinates (default: false)
* `ox {number}`          x origin (default: 0)
* `oy {number}`          y origin (default: 0)
* `theta {number}`       starting angle (default: 0)
* `radius {number}`      starting radius (default: 0)
* `dtheta {number}`      delta theta (default: 0)
* `dradius {number}`     delta radius (default: 0)
* `ddtheta {number}`     delta delta theta (default: 0)
* `ddradius {number}`    delta delta radius (default: 0)

Try this simple effect, which pulses particles away from and toward the center:

~~~
var particleObjects = this.pEngine.obtainParticleArray(10);
for (var i = 0; i < 10; i++) {
  var pObj = particleObjects[i];
  pObj.polar = true;
  pObj.dradius = 400;
  pObj.ddradius = -200;
  pObj.theta = i * 2 * Math.PI / 10;
  pObj.ttl = 5000;
  pObj.width = 20;
  pObj.height = 20;
  pObj.image = 'resources/images/sparkle.png';
}
this.pEngine.emitParticles(particleObjects);
~~~

NOTE: when using polar particles, `dx`, `dy`, `ddx`, and `ddy` translate the polar origin point.


## Triggers

Triggers allow you to attach any number of callbacks to a particle. They fire when a certain particle property reaches a certain value, as specified per each trigger.

* `property {string}`     check this property's value for trigger
* `value {number}`        value at which to trigger an action
* `smaller {boolean}`     if true, check if a property is < value, otherwise >
* `action {function}`     called when trigger occurs, passes particle
* `count {number}`        removes trigger after it has occurred count times (optional)

For example, a trigger that causes particles to bounce off of a certain y-value, as if it were solid ground, might look like this:

~~~
pObj.triggers.push({
    property: 'y',
    value: 500,
    smaller: false,
    action: function(particle) {
        // bounce the particle!
        particle.style.y = 500;
        particle.pData.dy = -particle.pData.dy / 2;
    }
});
~~~

NOTE: you can access and modify a particle's data object at any time via its `pData` property.