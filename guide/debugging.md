# Debugging with Chrome Web Inspector

The Hashcube DevKit leverages the power of [Google Chrome's](http://www.google.com/chrome) built-in web inspector to accelerate HTML5 game development.  This guide will help you get started using the major features of the Web Inspector.

## Opening Web Inspector

Right-click on a web document opened in the Chrome browser and select "Inspect Element".  The Web Inspector toolkit will pop up at the bottom of the browser window.

In any of the Web Inspector tabs you can undock the Inspector by clicking the lower-left Undock button.  The next button over will pop up a Console pane.

## Tab: Sources

In the sources tab there is a left navigator pane, a central document pane, and a right debugging pane.  At the bottom you will find the Pause-on-Exceptions toggle and Pretty Print.  Mouse-over and wait for details on what the buttons do.

The Pause-on-Exceptions toggle looks like a pause button and is tri-state:

+ Black: Disabled - Will not break automatically on exceptions
+ Blue: Break on All exceptions
+ Purple: Break on Uncaught exceptions

Pretty Print will reformat the JavaScript document for readability.  Because this is your own code, you probably do not need to use this feature.  When you build with the GC DevKit in release mode the scripts will get minified and will be hard to read, so it may be useful there.  Ideally however you will be debugging the debug-mode code that is not minified.

The navigator pane has Sources and Content scripts tabs.  Your game code is under the Sources tab under "./src".  The source code for all of the SDK JavaScript can also be seen in the Sources tab.

Set a breakpoint on a line of code in the central document pane by left-clicking on the line number.  It will appear in the Breakpoints list on the right, where you can uncheck it to disable it later.

## Tab: Console

The JavaScript code can add logs to the console with console.log().  This can be used for debugging, through breakpoints are preferable.  Errors will appear on the console in red, and you can filter the view by Errors, Warnings, and Logs.

To inspect global objects while not at a breakpoint, select the "<top frame>" drop-down and select "Simulator_0 (127.0.0.1)", and type the name of the global variable into the console.  You can click on the result object to view members if it is a complex type.

## Tab: Profiles

There are two major features in the Profiles tab:

+ CPU Profiling: "Collect JavaScript CPU Profile"
+ Heap Profiling: "Take Heap Snapshot"

These are both exciting features with very different goals.

### CPU Profiling

The CPU Profiler is used to help optimize your code.  A common use case would be that your app is running too slowly, so you want to determine where the bottleneck is located so that you can optimize that part of the code.

Play your game up to the point where it gets slow, then select "Collect JavaScript CPU Profile" from the Profiles tab.  Click [Start] at the bottom to start profiling.  When you're done with the slow part of your game, click [Stop] at the bottom.  Click the new recording item on the left.

What you will see is an interactive list of the functions written in your game, sorted by how much CPU time is spent in each one.  The functions at the top of the list are likely where the bottleneck lies.

Usually if an SDK function is the bottleneck then there is a more efficient API you could be using, so please read through [the API docs](../api/appengine.html) for a solution.  For optimizing your own functions, check out the [Performance Best Practices guide](./performance.html).

### Heap Profiling

The Heap Profiler is used to find memory leaks.  To use it, select "Take Heap Snapshot" and click [Start].  With a heap snapshot selected, you can click the record button on the bottom toolbar to record a second snapshot some time later.  By selecting the Comparison mode instead of Summary on the bottom toolbar you can see the types of objects that were created since the last snapshot.

A common type of leak is leaving references to views in callbacks, arrays, and other places where they can be held.

## References

[1] Chrome Developer Tools [https://developers.google.com/chrome-developer-tools/](https://developers.google.com/chrome-developer-tools/)
