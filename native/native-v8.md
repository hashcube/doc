# Google V8

## Overview

[Google V8](http://code.google.com/p/v8/) is a high-performance JavaScript JIT compiler and execution engine.  The Hashcube DevKit leverages this powerful engine to deliver realtime games on Android cellphones.

A lot has been written about how to develop efficient code to run on this engine.  Please see our [performance guide](../guide/performance.html) on this topic for considerations specific to the Hashcube DevKit, and for the best references on general performance with this engine.

To see how SpiderMonkey fits into the native stack on iOS, see the [Native Stack Overview](./native-stack.html).

## Hashcube Improvements

The V8 codebase is exceptionally well-suited for use on cellphones in realtime HTML5 games.  So we have not had to make many modifications for use in the Hashcube Android engine.

Hashcube has contributed back to the V8 project to smooth out the remote JavaScript debugging process: See AppSpot code review issue [6230045](https://codereview.appspot.com/6230045/) and [6305051](https://codereview.appspot.com/6305051/).
