# Loading Assets

## Module: ui.resource.loader

`ui.resource.loader` is a module used for preloading image and
sound assets.

Usage:

~~~
import ui.resource.loader as loader;
~~~

Loading assets, while asynchronous, is still demanding on the
system. When loading assets for a separate game screen, the
typical flow might be to queue up an animation to a loading
screen first, transition to the loading screen, then preload
the assets in the background. After the assets have been
preloaded, the callback is executed. Where a transition out of the loading screen, and to the game screen, can be triggered. 

Reasoning for this is that loading assets at the same time as presenting main menu screen, or during gameplay can produce undesirable visual artifacts (such as assets not being drawn on the screen for several frames while they load). By preloading assets within loading screens, smoother gameplay in general can be attained.

For the following loading methods, we'll assume the
following directory structure for a project:

~~~
project/
├── manifest.json
├── sdk -> /path/to/devkit/sdk
├── resources/
│	 ├── images/
│	 │	  └── bgimage.jpg
│	 └── sounds/
│	 	  ├── effects/
│		  │    └── boing.mp3
│		  └── music/
│		       └── levelmusic.mp3
└── src
	 └── Application.js
~~~

### loader.preload (path, callback)
1. `path {string}`
2. `callback {function}`

The `path` parameter is specified as a file path, or a
directory path containing multiple assets. When the sound
and image files are loaded into memory, the supplied
callback function is executed.

~~~
loader.preload('resources/images/bgimage.jpg', function () {
  //image is loaded ...
});
~~~

### loader.preload (paths, callback)
1. `paths {array}` --- A collection of path strings.
2. `callback {function}`

Multiple resource paths and/or files can be loaded at once by passing an array
of paths and/or files.

To load all the asset files contained in the directories and
sub-directories of `resources/images` and `resources/sounds`:

~~~
loader.preload(['resources/images', 'resources/sounds'], function () {
  //image is loaded, audio files are loaded ...
});
~~~
