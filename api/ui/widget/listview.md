# Class: ui.widget.ListView

Inherits from
:    1. [ui.ScrollView](./ui-scrollview.html)
     2. [ui.View](./ui-view.html)
     3. [event.Emitter](./event.html#class-event.emitter)

`ListView` renders a list of items using a number of
optimizations. If you need to display a scrollable list
containing a large number of items, consider using
`ListView`. `ListView` is a subclass of
[ScrollView ](./ui-scrollview.html), which allows list items
to be scrolled vertically.

`ListView` is visually similar to a `ScrollView` with a
[linear layout](./ui-view.html#style.layout). Both can
accomplish the same structure, positioning views in a
top-to-bottom layout. However, rendering the `ScrollView`
requires creating and positioning each subview. When the
layout contains subviews for dozens of items, this becomes
very expensive. `ListView` targets the case when these
subviews all have the same height, implementing several
optimizations that make this scenario performant.

## Examples

* [Basic ListView](../example/ui-list-basic/).
 
## Features

* **Viewport optimization**: Only the items within the
    visible region of the `ScrollView` are created,
    positioned, and rendered.

* **Subview recycling**: `ListView` manages the creation and
	deletion of its subviews. Each subview is an instance
	of a [CellView](#class-ui.widget.cellview). When a new
	item scrolls onto the screen, rather than initializing
	a new `CellView` instance, the `ListView` will reuse an
	existing, off-screen `CellView`.

* **Efficient updating**: Each `ListView` is backed by a
	[GCDataSource](#class-gcdatasource), which is a collection
	of data items. Each item is a JavaScript object that contains
	all the data necessary to render a single item in the
	list. When items are added or removed from the data
	model, the `ListView` automatically adds or removes
	`CellView` to the view hierarchy if those `CellViews`
	are on the screen.

## Usage Notes

When a `ListView` first attempts to render a cell to the screen, it will
invoke the `getCell` function, which you provide as an `opts` property to the
`ListView` constructor. Your `getCell` function must return an instance or
subclass of the [CellView](#class-ui.widget.cellview) type. If all the cells
will be of the same type, you can provide the constructor to the `ListView`
instead using the `cellCtor` property of the constructor `opts`.

Because subviews are recycled, `CellView` instances must be able to render an
arbitrary data item. When an item scrolls out of the visible boundary of the
list and the [CellView](#class-ui.widget.cellview) instance is recycled, the
`ListView` calls the `setData` function of the cell with the new data item. By
overriding this function, you can update the contents of the `CellView` to
reflect the new item.

The height of the `ListView` may be fixed externally, causing the `ListView`
to scroll if necessary. Alternatively, if `autoSize` is set to `true`, the
`ListView` sets its own height to the size of its cells, disabling scrolling.
This setting does not affect the rendering optimizations.

## Performance Notes

By only rendering `CellView` on the screen, the `ListView` achieves constant-
time layout and rendering (assuming a relatively small number of items can be
seen on the screen). By reusing cells, the `ListView` avoids garbage
collection and excessive object allocation during scrolling, creating at most
2x the maximum number of visible views.

A `ListView` may also be used with dynamically sized cells by setting
`isFixedSize` to false. However, this disables all relevant rendering
optimizations. This can still be useful for rendering a `GCDataSource` that has
frequent item modifications.


## Methods

### new ListView ([options])

Parameters
:    1. `options {object}`
     * `autoSize {boolean} = false` ---If `true`, automatically set the height of the `ListView` to the sum of its cell heights, disabling scrolling.
     * `dataSource {GCDataSource}` ---The data model. The [GCDataSource](#class-gcdatasource) provides the data for each item in the list, tracking adds, removes, and updates.
     * `getCell {function(listItem, itemResource)}` ---A function that returns a `CellView` instance given an item from the list.
     * `listItem {object}` ---An object representing the current list item.
     * `itemResource {object}` ---The resource object for the current list item.
     * `isFixedSize {boolean} = true` ---If `true`, enable optimizations based on the assumption that all cells have the same size. Set to `false` to allow variable sizes for list cells (hurts rendering performance).
     * `maxSelections {number} = 1` ---Maximum number of selectable items at a time.
     * `recycle {boolean} = true` ---If `true`, reuses `CellView` objects when scrolling rather than creating a new `CellView` for each item in the list. Has no affect if `isFixedSize` is `false`. (optimization flag)
     * `renderMargin {number} = 0` ---The vertical margin between list items.
     * `selectable {string} = null` ---If `single`, make a single item selectable, if `multi` allow multiple items to be selectable.
     * `selections {array} = null` ---If defined, select cells identified by keys in array.
     * `sorter {function(listItem)}` ---A function that returns a cardinal value (number or string) for the current item to use as the sort key.

~~~
import ui.widget.ListView as ListView;

var listview = new ListView({
  getCell: function (listItem) {
    return new CellView(listItem);
  },
  sorter: function (listItem) {
    return listItem.getName();
  }
});
~~~

### updateOpts (opts)

Parameters
:    1. `opts {object}` ---Allows updating constructor `opts` after the `ListView` has been created.

~~~
listview.updateOpts({
  dataSource: contacts.toDataSource()
});
~~~

### getCells ()

Returns
:    1. `{array}` ---Cell instances which are contained in the list.

### getSelection ()

Returns
:    1. `{Object}`

Gets the selected items from the list. Returns an object
with the keys being the selected items unique ids.

### getSelectionCount ()

Returns
:    1. `{number}` ---Returns the number of selected items.

### deselectAll ()

Deselect all cells in the list.


# Class: ui.widget.CellView

Inherits from
:    1. [ui.View](./ui-view.html)
     2. [event.Emitter](./event.html#class-event.emitter)


The `CellView` is the basic class for displaying items in a
list. The `CellView` class has functions for selecting,
deselecting and keeping track of selecting items.

When a cell becomes visible in a list, the `setData`
function is called. The reference to data can be stored in
the cell for later use.

~~~
this.setData = function (data) {
  this._data = data; // Store the reference to the data
  // Apply the data to the cell by updating a color, text, etc....
};
~~~

## Cell Selection

The `CellView` handles most selection logic
internally. There are however two functions which have to be
implemented to update the view when the cell is selected or
deselected: `_onSelect` and `_onDeselect`.

~~~
this._onSelect = function () {
  // Called when the cell is selected
  // Change something in the cell like the color, text, etc...
};

this._onDeselect = function () {
  // Called when the cell is deselected
  // Change something in the cell like the color, text, etc...
};
~~~

## Creating Cells

Cells should only be created in the `getCell` function,
which is invoked by `ListView` whenever it needs a new
cell. Adding and removing cells from the list manually will
lead to strange behavior!


## Methods

### new CellView ([options])

Parameters
:    1. `options {object}`

~~~
import ui.widget.CellView as CellView;

var cellview = new CellView();
~~~

### getWidth ()

Returns
:    1. `{number}` ---Returns `{number}` the width of the `CellView`.

### getHeight ()

Returns
:    1. `{number}` ---Returns `{number}` the height of the `CellView`.

### isSelected()

Returns
:    1. `{boolean}` ---Returns `{boolean}` if the current cell item has been selected.

### select()

Select the current cell item.

### deselect()

Deselect the current cell item.

## Events

### setData (data)

Parameters
:    1. `data {object}` ---Data representing the cell item to set.

When a list item scrolls into the list's visible area, the cell instance must
be updated with the item's data so that it displays the correct information.
The `setData` callback function is the mechanism by which this happens. Moreover, if
the list items are selectable, it's important to also update the cell state
based on its selected state. The `isSelected` function is the appropriated way
to check selected state.

~~~
this.setData = function (data) {
  this._data = data; // Save data

  // Update the view, e.g. set the text in a `TextView`
  // You can call `this.isSelected()` to get the selected state
};
~~~


# Class: GCDataSource

A `GCDataSource` is a collection of items that adheres to the [observable
pattern](http://en.wikipedia.org/wiki/Observer_pattern). This means that any
time items are added, removed, or updated, the `GCDataSource` will publish an
appropriate event for the item(s) in question (see [below](#events) for a list
of available events). `GCDataSources` abstract the connection between UI and
data model changes. For example, when you have a UI element (like a
`ListView`) displaying a collection of items, tying the UI to the `GCDataSource`
lets you update individual data items and have those changes automatically
propagate to the UI without re-rendering the entire collection. If you
implement a UI backed by a `GCDataSource`, you can update individual UI items
based on the events the `GCDataSource` publishes.

## Methods

### new GCDataSource ([options])

Parameters
:    1. `options {object}`
	     * `key {string} = 'id'` ---Key of an item that provides a unique identifier for that item.
		 * `ctor {function(data)}` ---Optional constructor function for casting added items to the same type
		 * `reverse {boolean} = false` ---If `true`, sort data source in reverse order.
		 * `sorter {function(item)}` ---Function that returns a cardinal value (number or string) for the current item to use as the sort key.

Creates a `GCDataSource` object. The most important argument is the `key`
parameter, which specifies what property on each item is unique. If you do not
specify a key, the `GCDataSource` will default to the `id` property of the item.

You may also specify a sorter function which, when given an item from the
`GCDataSource`, returns the item's cardinal value. The sort order of the sorting
functionality may be specified using the `reverse` flag.

~~~
import GCDataSource;

var dataSource = new GCDataSource({
	key: 'id',
	ctor: function (data) {
		this.id = data.id;
		this.name = data.name;
	},
	sorter: function (item) {
		return item.name;
	}
});
~~~

In advanced use cases, it is sometimes useful to provide a constructor
function (`ctor`), which will be used to 'box' items added using
[`dataSource.add(items)`](#datasource.add-item) when an added item is not
already an instance of the `ctor`. In these cases, the 'unboxed' item will be
passed as the first argument, effectively. Both the unboxed and boxed item
must be identifiable with the same uid for their key. Consider the following
psuedocode:

~~~
dataSource.add:
    uid = item[dataSource.key]
    if dataSource does not already contain uid then:
		if item is not instanceof ctor then:
    		item = new ctor(item)
            assert(item[dataSource.key] == uid)
        add item
~~~

### setSorter (sortFunction)

Parameters
:    1. `sortFunction {function(item)}`

Sets the `sorter` function, which must return a cardinal
value (number or string) for the current item, and will be
used in the sorting logic.

For example, to set the sorting order to be based on an
item's name property, you might set the sorter function as
follows:

~~~
dataSource.setSorter(function (item) {
  return item.getName();
});
~~~

### add (item)

Parameters
:    1. `item {array|object}` ---Add an item to the data source. Multiple items can be added by combining them in an `array`.

Returns
:    1. `{array|object}`

Adds or updates an item in the dataSource. Add multiple
items to a dataSource by passing them in an array:
`dataSource.add([item1, item2, item3])`.

Returns `{array|object}` the item you passed in. Particular
useful if using the `ctor` option.

### remove (key)

Parameters
:    1. `key {string}` ---The key value of the item to be removed.

Returns
:    1. `{object}`

Removes and returns an item from the data source by its key value.

### clear ()

Remove all items from the dataSource. Note that this will
cause the `'Remove'` event to be published for all the items
currently in the list.

### contains (key)

Parameters
:    1. `key {string}` ---The key value of the item to search for in the datasource.

Returns
:    1. `{boolean}`

Return `true` if an item with the given key is contained within the datasource.


### get (uid)

Parameters
:    1. `uid {string}` ---The unique id of the item to return.

Returns
:    1. `{object}` ---Returns the item from the list with the given uid if one exists.

~~~
var item = dataSource.get(uid);

assert(item[dataSource.getKey()] === uid);
~~~

### getItemForIndex (index)

Parameters
:    1. `index {number}` ---The index of the item to return.

Returns
:    1. Return: `{object}`

Return the item with the given index based on the current sort order.

### forEach (callback [, thisArg])

Parameters
:    1. `callback {function (item, index)}` ---Function invoked for each item in the dataSource
     2. `thisArg {object} = undefined` ---Object to use as `this` when executing `callback`.

Iterator used to execute a function on each item in the
dataSource. For example, to log out the name of each item
using it's `getItem` method:

~~~
dataSource.forEach(function (item) {
  console.log("Item name: " + item.getName());
});
~~~

### getFilteredDataSource (filter)

Parameters
:    1. `filter {function (item)}` ---Function that tests each item in the dataSource for membership in the filtered `GCDataSource`.

Returns `{GCDataSource}` --- a new filtered dataSource with all the items from
the original dataSource that the callback function returns `true` for. The
filtered `GCDataSource` is _live_, meaning that adding or updating items in the
original `GCDataSource` at any time will execute the `filter` function to check
to see if the new or updated item should be in the filtered `GCDataSource`.
Similarly, if items are removed from the original `GCDataSource`, they are
removed from the filtered `GCDataSource`.

~~~
var Value = Class(function () {
  this.init = function (opts) {
    this.value = opts.value;
  }
});

var set = new GCDataSource({
  key: 'value',
  ctor: Value
});

var evenSet = set.getFilteredDataSource(function (item) {
  return item.value % 2 == 0;
});

set.add([
  {value: 1},
  {value: 2},
  {value: 3},
  {value: 4}
]);

evenSet.toArray().map(function (item) { return item.value; });
// => 2, 4, 6, 8, 10

~~~

### filter (filter)

Parameters
:    1. `filter {object}` ---Object of strings.

Returns
:    1. `{GCDataSource}` ---All items that match the filter.

Performs a basic string filter on the items in a `GCDataSource`, useful for text
searching. The returned `GCDataSource` is _not live_. For each key in the
`filter`, if the item contains the key and the corresponding value is a
`{string}`, the value must contain the corresponding filter string.

~~~
dataSource.add([
  {id: 1, color: 'orange'},
  {id: 2, color: 'magenta'},
  {id: 3, color: 'purple'},
  {id: 4, shape: 'circle'},
  {id: 5, shape: 'rectangle'}
]);

// only matches items with ids 3, 4, and 5
var filtered = dataSource.filter({color: 'le', shape: 'rect'});

filtered.toArray(); //=> [{id: 3, ...}, {id: 4, ...}, {id: 5, ...}]
~~~

### keepOnly (list)

Parameters
:    1. `list {array}` ---Array of items to include from this data source.

Remove items from the current datasource not in the given
list. This is essential the opposite of `datasource.filter`

~~~
datasource.add([item1, item2, item3, item4]);

datasource.keepOnly([item2, item3]);

datasource.toArray(); //=> [item2, item3]
~~~

### sort ()

Sorts the array based on the current sorter function.

### toArray ()

Returns
:    1. `{array}`

Returns an array of all the items in the datasource.

### toJSON ()

Returns
:    1. `{object}`
	     * `key {string}` ---The key of the datasource.
		 * `items {array}` ---An array representing all items in the datasource.

Returns a JSON object representing the current datasource.

### fromJSON (data)

Parameters
:    1. `data {object}`
	     * `key {string}` ---The key of the datasource.
		 * `items {array}` ---An array representing all items in the datasource.

Populates the current datasource using the given key and items.


## Properties

### key `{string}`

*Alias: `getKey()`*

The key used to retrieve a unique id for each item

### length `{number}`

*Alias: `getCount()`*

Return the number of items in the datasource.


## Events

### \'Update\', callback (key, item)

Parameters
:    1. `key {string}` ---The key value of the item being updated or added.
	 2. `item {object}` ---The item being updated or added.

The `Update` event is published when an item in the datasource has been added or updated.

### \'Remove\', callback (key, item)

Parameters
:    1. `key {string}` ---The key value of the item being removed.
	 2. `item {object}` ---The item being removed.

The `Remove` event is published when an item has been removed from the data source.
