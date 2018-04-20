# Troubleshooting Issues on Android

## Overview

When your game becomes unresponsive it may be due to a JavaScript exception.  Check the JavaScript logs (see below) to figure out what caused the problem.

## Using Android Devices

General knowledge of how to use Android devices is helpful while solving game problems:

### Uninstalling Games

To uninstall a game on Android:

1.  Open the Settings app.
2.  Select "Applications".
3.  Select "Manage applications".
4.  Select your game from the list.
5.  Tap [Uninstall] and tap OK.

### Force-Quitting Games

To force-quit a game on Android:

1.  Open the Settings app.
2.  Select "Applications".
3.  Select "Manage applications".
4.  Select your game from the list.
5.  Tap [Force Quit] and tap OK.

## Watching JavaScript Logs

To watch the JavaScript logs from a mobile device, connect it to your computer via a USB data cable, then use the `adb logcat` command at a terminal window.

Normally you will want to pair `adb logcat` with the standard `grep` tool to cut out most of the uninteresting log messages.  For example `$ adb logcat | grep JS` will primarily display messages from your game.

You may then pipe the output to a file with `$ adb logcat | grep JS > mylog.txt`.  Hit CTRL+C to terminate the log capture, and open it with `open mylog.txt`.

## Unable to Debug Android Device with ADB

Often times you will need to follow device-specific instructions to enable a USB debug mode in the device settings.

Another common problem is that some of the Android devices out there are not registered with Android's default Vendor list.  For example the Xiaomi Mi 2[S] phones or the Ouya game console.

To get them to work with ADB (and the DevKit) on MacOSX you have to go into "About This Mac", select "More Info" => "System Report", select USB on the left so you can see the attached USB devices.  On other platforms it is a similar process.  Select the device from the list.

You will see something like this:

~~~
MI 2:

  Product ID:    0x9039
  Vendor ID:    0x2717
  Version:     2.28
  Serial Number:    a5629fa
  Speed:    Up to 480 Mb/sec
  Manufacturer:    Xiaomi
  Location ID:    0xfa120000 / 5
  Current Available (mA):    500
  Current Required (mA):    500
  Capacity:    47.9 MB (47,876,096 bytes)
  Removable Media:    Yes
  Detachable Drive:    Yes
  BSD Name:    disk2
  Partition Map Type:    Unknown
  S.M.A.R.T. status:    Not Supported
~~~

The Vendor ID is what we need.  Add it to the end of your "~/.android/adb_usb.ini" file like this:

~~~
# ANDROID 3RD PARTY USB VENDOR ID LIST -- DO NOT EDIT.
# USE 'android update adb' TO GENERATE.
# 1 USB VENDOR ID PER LINE.
0x2717
~~~

Now run `adb kill-server` and `adb start-server` to restart ADB.

If you type `adb devices` you should now see your device in the list.  Also check the screen of the device and accept any access prompts that show up.

*Avoid running the `android update adb` command as it will erase your changes.*

## JavaScript Freezes

If your JavaScript game freezes up, it can be due to either an Exception, or a Memory Leak.  To figure out which problem it is, follow instructions above to watch the JavaScript logs.

If you see an exception trace in the log then it will contain information about which source file and line caused the error.  You may want to use the [remote JS debugger](./android-remote-debug.html) to set a breakpoint a little before the exception line so that you may fully inspect the code state from the comfort of your desktop.  You can also turn on break-on-exception to cause the debugger to halt at the line where the exception occurs.

If you see "JavaScript error out of memory" at the console then you probably have a memory leak.  The best way to debug this sort of issue is to back off to using just your browser for development.  Chrome has the best heap profiler available, and you can [use it](https://developers.google.com/chrome-developer-tools/docs/heap-profiling) to find and fix JavaScript memory leaks.

## Custom TrueType Fonts Not Working

On Android, it is crucial that custom .TTF file names match one of the names inside the font file.

Make sure that you have included the .TTF (TrueType Font) file under `./resources/fonts` in your game directory.  And that under `./manifest.json` you have a "ttf" section containing the relative path to the file, as in the following example:

~~~
"ttf": [
	"resources/fonts/Arial Black.ttf"
],
~~~

Inside your game you can reference the font by one of the names inside the font file.  For best interoperability, the .TTF file name should also match one of the names inside the font file.  For example:

~~~
var textview = new TextView({
	superview: this.view,
	layout: "box",
	text: "Hello, world!",
	fontFamily: "Arial Black",
	size: 50,
	verticalAlign: "top",
	color: "#00ffff",
	width: 500,
	height: 20,
	x: 200,
	y: 100,
	backgroundColor: "#ff7733"
});
~~~

## Device Communication Failures

The Android Debug Bridge (ADB) may be used to help debug problems with the Android USB connection.  First off, use ADB to list the connected devices at a terminal window:

`$ adb devices`

If you run into problems getting your device to work, first unplug the USB cable and replug it in.  If that does not fix the problem, then try to stop the ADB server on your computer:

`$ adb kill-server`

Also attempt to reboot the mobile device if communication is still unsuccessful.

Further references:

* [Using Hardware Devices](http://developer.android.com/guide/developing/device.html)
* [Android Debug Bridge](http://developer.android.com/guide/developing/tools/adb.html)

## EMFILE Release Build Failures

The release builds open a lot of files during the JavaScript code compression step.  If you see "Error: EMFILE, too many open files" in your build log then you should run this command at a console: `sudo ulimit -S -n 2048` to increase the open file limit.
