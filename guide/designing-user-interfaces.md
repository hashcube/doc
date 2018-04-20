# Designing User Interfaces

When building game UI in `timestep`, we want to be able to
layout views that flow without concern for the screen dpi,
aspect ratio, or resolution.

The first step to building UI is setting up the viewport.
This is non-trivial because the viewport changes depending
on the screen size.  There are a few different methods for
configuring your viewport.  To understand these methods, you
must first understand _aspect ratio_.

## Aspect ratio

The *aspect ratio* is the ratio between the width and the
height of a screen resolution.  The aspect ratio is
important because a UI designed for one screen resolution
trivially scales to fit any screen resolution with the same
aspect ratio.  By trivially we mean that it fills the screen
entirely without clipping any content.

Two different screen resolutions can have the same aspect
ratio.  For example, the squares `100x100` and `200x200`
have the same aspect ratio, `1:1`, while the HD 1080p
resolution of `1920x1080` shares the aspect ratio, `16:9`,
with the iPhone 5's resolution of `1136x640`.  We refer to
the aspect ratio as the smallest integer fraction that
represents the ratio.


## Fixed aspect ratio: stretch to fit

If you're only designing for one aspect ratio, it's
relatively easy to design your UI.  For example, only
targetting the iPhone 5 and other `16:9` devices would allow
you to design for a `16x9` resolution such as `1024x576` and
scale your content to fit any `16x9` screen.

As soon as you want to fill the screen on all devices,
however, you need to code your UI with some flexibility to
handle the differences in aspect ratios.  One method is to
still target a fixed aspect ratio, but handle clipping
differently.  There are three common ways to scale a fixed
aspect ratio:

1. _cover_: The notion of covering a screen means that all
   available pixels on the screen show the game (no "black
   bars").  We don't want to squish or stretch the screen, so
   we scale the screen to fill all the pixels and clip
   anything that expands beyond the screen boundaries.  In
   practice, it's usually fine to clip background images and
   simply align the important UI elements with the edges of
   the screen. An example where _cover_ makes a lot of sense is an
   isometric simulation game.  Here, having different aspect
   ratios simply means that users can see more or less of
   the game map.  UI controls snap to the sides of the
   screen.  Users with higher aspect ratios will see more of
   the game world than users with lower aspect ratios.  In
   multiplayer games, this may lend itself to a competitive
   advantage, in which case another scaling method might
   make more sense.

2. _contain_: The notion of containing a screen means that
   all pixels in the original design are visible. We scale
   the screen as large as we can without clipping any
   pixels. When the aspect ratios are not equivalent, this
   leads to bars on the side. We can ignore these, resulting
   in black bars, or fill them with some repeating pattern
   or texture.

3. Hybrid: UI designers may implement a combination of cover
   and contain.  Usually, this is done by prefering _cover
   over contain_, choosing a maximum amount of clipping that
   is allowed for cover.  If covering would result in too
   much clipping, the UI is first scaled using `cover` to
   the maximum allowed clip and then scaled using `contain`
   to ensure that all important visual regions are not being
   clipped.


## Fixed DPI

For UI-heavy games, you may want to scale your UI based on
dpi, but then dynamically position all subviews according to
the screen widths.  For background images, you could either
apply the `cover` or `contain` methods directly to the
images or use a scaling method based on slices (see
ScaledImageView).


## Automatic Subview Layout

For either the fixed dpi or fixed aspect ratio methods, you
will want to impose higher-level positioning constraints on
your views.  In a game, you may only care about the `x`,
`y`, `width`, and `height` of a view.  The game logic
determines exactly where each subview is.  Consider an army
marching across a field: a developer would probably use a
subview for each army unit with a controller class setting
each subview's position based on an underlying data model.
but in a UI, you frequently want to position views relative
to each other.  For example, "place this image 20px from the
bottom edge of the screen," or, "put these images in a line
with 5px of space between each image."

Each view in timestep can contain subviews. A UI composed of
several buttons or input fields, however, can quickly become
tedious to layout by hand.

You typically set the position of a basic timestep view by
setting its `x`, `y`, `width`, and `height`.  Layouts define
higher-level rules that control some (or all) of these
attributes for you.


## Box Layout

The `box` layout is a collection of the most basic layout
positioning rules.  Most other layouts extend from the `box`
layout -- for example, a view with a `linear` layout can be
positioned using any of these properties as well.

Compared to more advanced layouts such as `linear`, the
`box` layout does not control the layout of its subviews.


### Centering

If you want to center multiple views, you should consider
using `justifyContent: center` in a `linear` layout, but you
can use the `centerX` and `centerY` properties for aligning
a single view in the parent view.

<style>
.figure { border: 1px solid #888; width: 300px; height: 350px; background: #EEE -webkit-linear-gradient(270deg, #CCC, #EEE); 
	box-shadow: inset 0px 2px 5px rgba(0, 0, 0, 0.2);
    border-radius: 3px
}
.figure-phone {
  width: 170px; height: 280px; margin: auto; border-radius: 10px; margin-top: 30px; background: #444; border-style: solid; border-color: black; border-width: 1px;
  background: -webkit-linear-gradient(280deg, #333, #666 50%, #000 51%, #444);
  box-shadow: 1px 4px 10px rgba(0, 0, 0, 0.5);
}
.root-view { margin: 20px auto; width: 160px; height: 240px; background: rgba(200, 200, 230, 0.9); border-radius: 2px; }
.centered-view {  width: 50px; height: 50px; margin: 0px auto;
  -webkit-animation-name: expandIt;
  -webkit-animation-iteration-count: infinite;
  -webkit-animation-duration: 5s;
  -webkit-animation-timing-function: linear;
  -webkit-transform-origin: center top
}
.boxLayout .centered-view {
    position: relative;
    top: 20px;
	background: blue;
}
    
@-webkit-keyframes expandIt {
  0% {
  }
  50% {
    width: 100px;
    height: 100px;
  }
  100% {
  }
}
.caption {

}
</style>
<div class="figure">
	<div class="figure-phone">
    	<div class="root-view boxLayout">
        	<div class="centered-view">
            </div>
        </div>
    </div>
</div>
<div class="caption" markdown>
Figure 1: centered view with `{y: 20, centerX: true}`.
</div>


### Offset alignment

The propeties `top`, `left`, `right`, `bottom` specify the
distance from the corresponding edge of the parent view.
The layout computes (and overwrites) values for `width`,
`height`, `x`, and `y` based on these properties.  When the
superview resizes, these properties automatically update the
view's position.

If both `top` and `bottom` are set, the view is stretched to
meet both constraints (overriding any preset `height`).
Similarly, if both `left` and `right` are set, the `width`
is ignored.


### Percentage width and height: layoutWidth and layoutHeight

**Note: the following also applies to `height` and `layoutHeight`.**

You can apply constraints to the `width` of a view using
`layoutWidth`.  Setting `layoutWidth` of a view to a
percentage value, e.g. `"80%"`, will constrain the view to
80% of the superview's width.

You can also assign the constant `"wrapContent"` to
`layoutWidth`, which fits the width of the view to the sum
of its children.


### Fixed aspect ratios

Sometimes you want to control one dimension of a view, while
fixing the other dimension to a particular ratio of the
first.  To do this, set the `aspectRatio` property of a
view.  We define `aspectRatio` as the ratio `width /
height`.

If you want the `aspectRatio` to be computed automatically,
set `style.fixedAspectRatio` to `true`.  Any `ImageView`
with `autoSize: true` and `fixedAspectRatio: true` will
update the aspect ratio automatically when the image is
auto-sized.


## Linear Layout

The `linear` layout extends from the `box` layout.

One of the most common representations of a layout is a
line.  This encapsulates everything from a high-level
structural layout such as a header, body, and footer
(vertical line) to a more detailed layout such as a list of
buttons in a toolbar (horizontal line).  Linear layouts can
be easily nested to build more complex UI.

The `LinearLayout` class makes configuration of linear lists
of views very easy.  To change the layout of a view, set the
`layout` property of a view:

Programatically:

~~~
var view = new View({
  layout: 'linear',
  direction: 'vertical'
});

var redview = new View({
  superview: view,
  backgroundColor: 'red'
});

var blueview = new View({
  superview: view,
  backgroundColor: 'blue',
  flex: 1
});

var greenview = new View({
  superview: view,
  backgroundColor: 'green'
});
~~~

Or using JSON:

~~~
var MainView = Class(View, function (supr) {
  this._def = {
    layout: 'linear',
	children: [
	  {backgroundColor: 'red'},
	  {backgroundColor: 'blue'}
    ]
  };  
});
~~~


### Layout Order

Subviews are ordered by `zIndex` first and add-order second.
This means that if the `zIndex` values for two subviews are
equivalent, the two views are sorted by the order in which
they were added, with later views appearing last.  Setting
`zIndex` can therefore resort the subviews.

Sometimes, you need to layout views in a different order
than the underlying sort order.  For example, in a vertical
layout with a fixed header and a scrolling body, you may
want the header positioned above the body so that it can
overlap the body content.  Setting the header's `zIndex`
will change the order of the subviews.  If the header has a
higher `zIndex`, the body renders first and the header
renders second, placing the header _above_ the body.
However, you still want the header to be positioned first in
the linear layout.

To specify a sorting for the layout order that differs from
the subview order, set the `order` property on a subview's
style.  Higher `order` values will be later in the layout.
The `order` defaults to `0` and should always be an
integer.


### Ignoring views in the layout

Sometimes you want some, but not all, subviews to be in your
linear layout.  By default, all subviews are in a linear
layout.  To remove a view from the layout, set the
`inLayout` property of a subview's style to `false`.


### Sizing the containing view to fit

This can be accomplished using the same `layoutWidth` and
`layoutHeight` values inherited from the box layout.


### Margin

A view in a `linear` layout can specify margin using the
style properties `top`, `left`, `bottom`, and `right`.  This
adds space around each view when they are layed out.


### Padding

A view with a linear layout can add `padding` using the
`style.padding` property.  `style.padding` is an instance of
`ui.layout.Padding`.  You can assign directly to it with a
number or string, such as:

~~~
// 10px padding on all sides
view.style.padding = 10;

// 10px padding on top/botom
// 20px padding on left/right
view.style.padding = "10 20";

// 10px on top, 20px on right
// 30px on bottom, 40px on left
// (clockwise set)
view.style.padding = "10 20 30 40";
~~~

You can also address the four sides of padding independently:

~~~
view.style.padding.top = 10;
view.style.padding.right = 30;
view.style.padding.left = 40;
view.style.padding.bottom = 20;
~~~


### Flex

A view in a layout can specify a dynamic resize attribute
`flex`.  This can only be used if the containing view has a
fixed dimension (the layout does not have `layoutWidth` set
to `wrapContent`).

A view with `flex` set to a nonzero value will expand to fit
the available size.  It will not shrink below its
`minWidth`, if one is set.  It will also not exceed
`maxWidth`.


### Justify Content

If, after laying out all subviews, there is space left over,
you can choose to distribute that space between the views in
different ways.  Set the `justifyContent` property of a view
to one of the following:

* `start`: the default value, this does what you would
   expect for the default behavior - views are laid out in
   order from the start and all space is left at the end of
   the line.
* `end`: The extra space is pushed to the front of the line.
* `center`: The space is distributed between the start and
  end of the line.  This effectively centers the subviews!
* `space`: Space is distributed evenly between all gaps in subviews.
* `space-outside`: Space is distributed evenly between all
  gaps in subviews as well as at the start and end of the line.

<style>
.justifyCentered { background: none; position: relative; top: 20px; }
.justifyCentered.hWrapContent { border: 1px solid red; border-radius: 2px; }
.hWrapContent .centered-view { border: none !important; padding: 10px 0px; position: relative; }
.justifyCentered .subview { top: 5px; bottom: 5px; display: inline-block; border-radius: 2px; }
.justifyCentered .subview:first-child {
	position: absolute;
    left: 5px;
    padding-right: 3px;
    right: 50%;
}

.subviewblue {
	background: blue -webkit-linear-gradient(270deg, rgb(0, 0, 255), rgb(0, 0, 200)); 
    height: 100%;
}

.justifyCentered .subview:last-child {
	position: absolute;
    width: 50%;
    right: 5px;
    padding-left: 8px;
}

.justifyCentered .centered-view { border: 1px solid green; position: relative; border-radius: 2px; }

</style>

<div class="figure">
	<div class="figure-phone">
    	<div class="root-view">
        	<div class="justifyCentered hWrapContent">
              <div class="centered-view">
                  <div class="subview"><div class="subviewblue"></div></div>
                  <div class="subview"><div class="subviewblue"></div></div>
              </div>
            </div>
        </div>
    </div>
</div>
<div class="caption" markdown>
Figure 1: centered view with `{y: 20, justifyContent: 'center', layoutHeight: 'wrapContent', layoutWidth: 'wrapContent'}`.
</div>

<div class="figure">
	<div class="figure-phone">
    	<div class="root-view">
        	<div class="justifyCentered">
                <div class="centered-view">
                  <div class="subview"><div class="subviewblue"></div></div>
                  <div class="subview"><div class="subviewblue"></div></div>
                </div>
            </div>
        </div>
    </div>
</div>
<div class="caption" markdown>
Figure 1: centered view with `{y: 20, justifyContent: 'center', layoutHeight: 'wrapContent', layoutWidth: 'wrapContent'}`.
</div>
