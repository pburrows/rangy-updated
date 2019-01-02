
[[Documentation home|Home]]

# Selection save and restore module

This module saves and restores a user selection using invisible marker elements in the DOM. This is useful in situations where the selection may be lost for some reason, such as when altering large portions of the DOM or using `innerHTML` to alter and replace the contents of an element. So long as the marker elements (or new elements with their `id`s) are still present, the selection can be restored.

There is a limitation to this method, which is that due to it being impossible to programmatically create a backwards selection in IE, the backwardsness of a selection is is always lost when it is restored using `rangy.restoreSelection()`.

You can see a live example of this module in action [here](http://rangy.googlecode.com/svn/trunk/demos/saverestore.html).

## Example

Imagine the following HTML containing a selection (denoted below by | characters):

```
<p>
    A cup of |tea and a <b>slice| of cake</b>, please.
</p>
```

You can save the selection using this Rangy module as follows:

```
var savedSel = rangy.saveSelection();
```

This action adds hidden marker elements into the DOM. An HTML representation of the DOM would now look something like

```
<p>
    A cup of <span id="x"></span>tea and a <b>slice<span id="y"></span> of cake</b>, please.
</p>
```

The `id` of these marker elements (`x` and `y` above) are actually randomly generated to be unique.

Now, imagine something happens to this content that will destroy the selection and potentially change the DOM. Provided the marker elements have survived, you can later restore the old selection:

```
rangy.restoreSelection(savedSel);
```

This will also remove the marker elements.

If you've saved a selection but do not need to restore it, you can remove the marker elements:

```
rangy.removeMarkers(savedSel);
```

## API

### Note about documentation formatting

Square brackets around one or more function or method parameters indicate that the parameter(s) is/are optional.

----

#### `rangy.saveSelection([Window win])`

Saves the current selection for the specified window by adding hidden marker elements into the DOM at the beginning and end of the selection (or a single marker element if the selection is an insertion point or caret). If no window is specified, the current window is used.

Returns an object representing the saved selection that may later be passed to `rangy.restoreSelection()`.

*Note*

From version 1.1.2, if this method is called in Internet Explorer for an iframe that has lost the focus and previously had a collapsed selection (caret), the method will abort and return null. Previously, this method would insert a marker in the main document rather than the iframe in such circumstances, which is clearly undesirable. To avoid this happening, call the `focus()` method on the iframe's window before calling `rangy.saveSelection()`. This is due to a quirk in Internet Explorer and doesn't affect any other browser. See issue 46 for a discussion.

----

#### `rangy.restoreSelection(Object savedSelection[, Boolean preserveDirection])`

Restores the selection from an object previously returned by `rangy.saveSelection()`. It removes any hidden marker elements created by that method.

If `preserveDirection` is supplied and is `true`, the direction of the selection is preserved. However, this will only work in browsers with a native `Selection` object that supports the [extend()](https://developer.mozilla.org/en/DOM/Selection/extend) method, meaning this parameter will have no effect in IE.

When the selection being restored contains only a single range, this method calls [normalizeBoundaries()](RangyRange#normalizeBoundaries()) on the range being restored. If the selection contains multiple ranges, no boundary normalization is performed.

----

#### `rangy.saveRange(Range range)`

##### From v1.3

Saves the current range by adding hidden marker elements into the DOM at the beginning and end of the selection (or a single marker element if the selection is an insertion point or caret).

Returns an object representing the saved range that may later be passed to `rangy.restoreRange()`.

----

#### `rangy.restoreRange(Object savedRange[, Boolean normalize])`

##### From v1.3

Restores a range from an object previously returned by `rangy.saveRange()`. It removes any hidden marker elements created by that method.

`normalize` is optional and controls whether `normalizeBoundaries()` is called on the restored range. The default is `true`.

----

#### `rangy.saveRanges(Array ranges[, String direction])`

##### From v1.3

Saves the current range by adding hidden marker elements into the DOM at the beginning and end of the selection (or a single marker element if the selection is an insertion point or caret).

`direction` indicates a direction to be stored with each saved range and may be any of the strings "forward", "forwards", "backward" or "backwards" or a Boolean (in which case true corresponds to "backwards").

Returns an array of object representing the saved ranges that may later be passed to `rangy.restoreRanges()`.

----

#### `rangy.restoreRanges(Array savedRanges)`

##### From v1.3

Restores an array of ranges from an object previously returned by `rangy.saveRanges()`. It removes any hidden marker elements created by that method.

----

#### `rangy.removeMarkers(Object savedSelection)`

Removes the hidden marker elements associated with a saved selection object previously returned by `rangy.saveSelection()`. This saved selection can then no longer be restored.