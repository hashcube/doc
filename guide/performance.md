# Performance Best Practices

## Overview

A smooth framerate in your game is essential for a good user experience.  The Hashcube DevKit allows you to write your game code rapidly in JavaScript software, which will then also run accelerated on mobile hardware.

Reaching your performance goals requires keeping a few key concepts in mind while coding.  JavaScript is a garbage-collected language, meaning that periodically all of the disposed objects need to be collected and freed so the memory can be reused.  This garbage collection can cause frames to drop if a large number of objects are being created and freed by your game.

Rapid development is a top priority, and you can always go back and apply the following techniques after your game is up and running to improve performance further.  Testing your game on lower-end phones such as the iPhone 3GS will help you determine how much time needs to be spent actually working on performance issues.

## Preload resources

Resource loading can be hard on a game's framerate. Make
sure to preload any assets before you need them to prevent
noticeable in-game lags.  This also prevents game assets
from "blinking in" after gameplay starts.

Example preloader usage:

~~~
import ui.resource.loader as loader;

loader.preload(["resources/images/tutorial", "resources/sounds"], function() {
	console.log("Finished loading tutorial images!");
});
~~~

See the [asset loading guide](./load-assets.html) for more information on how to preload resources.

## Optimize Selectively

Any code that *can* be taken out of a loop, *should* be
taken out. Function calls carry some additional
overhead. Generally, modern JavaScript engines optimize for
most use cases, but using a tool like
[jsPerf](http://jsperf.com) to test snippets can be very
insightful (even though it run tests in your browser, the
lessons can still apply).

Also, get comfortable with debugging JavaScript using the
Chrome Developer Tools, especially the
[Profiles Panel](https://developers.google.com/chrome-developer-tools/docs/profiles),
with particular attention to the **CPU profiler** and the
**Heap profiler**.

While log messages are helpful to make development faster, avoid excessive logging to the console every frame for your final release.

## Manually trigger GC

Manually triggering garbage collection (GC) while waiting for user input or otherwise waiting at a menu or modal screen will prevent GC lag spikes from occurring during gameplay when they are noticeable.

Invoke `device.collectGarbage();` to force garbage collection. Note that this has no effect in web browsers.

## Allocate fewer objects

The more objects that are created, the greater the
performance lag when the JavaScript engine needs to garbage
collect them, and if there are any references to an object,
its memory won't be de-allocated. It's better to reuse
existing objects than create new ones, so for example:

* Use `Date.now()` instead of `+new Date()`.
* Clear an old array with `arr.length = 0` instead of creating a new one.
* Likewise, recycle objects rather than creating new ones.

There are some symptoms for bad garbage collection:

* Excessive logs about garbage collection (GC) from V8 (Android) in `adb logcat` or Spidermonkey (iOS) on the Xcode console.
* Irregular glitches in framerate when you're not doing anything notable in JavaScript.
* Smooth framerates punctuated with brief lag spikes.

### Use View pools

Since you take a performance hit when creating objects, it
can be beneficial to create "pools" of `View` objects ahead
of time, and then when your ready to use them, acquire them
from the already allocated pool. When you are done with the
view, release it back to the pool so it can be used again later.

This means that no garbage collection needs to be done at all for these heavy objects, reducing GC churn during gameplay.

## Native code is faster than JavaScript

JavaScript is fast, but it's even faster to let the native
runtime do the work. Calls to JavaScript have more
overhead than native code. The more code you can offload to
the native side, the faster your game will run.

## Use the animation engine

This is related to the previous tip. Using our
[animation engine](../api/animate.html) is faster than
calculating in a game loop because it can be optimized for
native execution. The less calculations in JavaScript, the better!

## Schedule tasks over multiple frames

Game calculations can be expensive, especially when done on
each frame. Many times, effects such as physics collisions
and AI and be run every other frame (or even less) without a
loss in visual quality. It's important to schedule these
tasks across animation frames for a consistent, and better
performing game.

## Recommended Reading

Most of the references you will find online are [V8-specific](../native/native-v8.html) tips, which have an effect on the Chrome Web Browser and the Android native platforms.  However, a subset of these best practices has a positive impact on performance in [SpiderMonkey](../native/native-sm.html) also, which will help on iOS devices.

[1] An excellent starting point: [http://www.html5rocks.com/en/tutorials/speed/v8/](http://www.html5rocks.com/en/tutorials/speed/v8/).

[2] For a more technical overview with good examples: [http://oreilly.com/server-administration/excerpts/even-faster-websites/writing-efficient-javascript.html](http://oreilly.com/server-administration/excerpts/even-faster-websites/writing-efficient-javascript.html).

[3] For a deep dive into technical information see Florian Loitsch's blog: [http://floitsch.blogspot.com/2012/03/optimizing-for-v8-introduction.html](http://floitsch.blogspot.com/2012/03/optimizing-for-v8-introduction.html).

[4] In particular, we have found that using Google D8 to inspect deoptimizations is a great way to profile your JavaScript code for performance improvements that can be gained through refactoring your code.  See this blog post: [An introduction to deoptimizations](http://floitsch.blogspot.com/2012/03/optimizing-for-v8-inlining.html).

[5] Finally: A 45 minute video from Google I/O 2012 on ["Breaking the JavaScript Speed Limit with V8"](http://www.youtube.com/watch?v=UJPdhx5zTaw)

