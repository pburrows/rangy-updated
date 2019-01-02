[[Documentation home|Home]] 

# Selection objects in Rangy

## Contents

  * [Introduction](#introduction)
  * [Getting hold of the selection](#getting-hold-of-the-selection)
  * [Working with a Rangy selection](#working-with-a-rangy-selection)
  * [Control selections in Internet Explorer section](#control-selections-in-internet-explorer)
  * [API: Properties](#properties)
  * [API: Methods](#methods)

## Introduction

The Selection object in Rangy provides a single API for manipulating the user's selection. The API is based on a set of properties and methods common to all major browsers except for IE <= 8. The aim is to provide only the functionality that will work in all target browsers. However, the situation for selections is considerably more complicated than that of Ranges:

 * The user selection is a part of the browser's UI and cannot be practically be overridden;
 * Browsers have significant variation in selection behaviour;
 * Some browsers have significant shortcomings in their selection implementations that cannot be worked around. For example, IE <= 8's selection object is completely different to that found in other browsers. IE and WebKit both have a serious issue that means they do not allow a collapsed selection (caret) to be placed at the start of an element ([#23189](https://bugs.webkit.org/show_bug.cgi?id=23189), [#15256](https://bugs.webkit.org/show_bug.cgi?id=15256); this shortcoming in WebKit also means that a Range added to the selection in WebKit comes out of the selection altered;
 * The only standard for Selection objects is in the [work-in-progress HTML Editing APIs specification](http://dvcs.w3.org/hg/editing/raw-file/tip/editing.html#selections). No mainstream browser yet conforms fully to this section of the specification, although all current major browsers come reasonably close in practice.

In general, if you are used to Mozilla, WebKit and Opera's selection objects then you'll be able to use Rangy's selection object. There are a few crucial differences, detailed [below](#Working_with_a_Rangy_selection).

You can see a live example of Rangy's selection objects in action [here](http://rangy.googlecode.com/svn/trunk/demos/core.html).

## Note about documentation formatting

Square brackets around one or more function or method parameters indicate that the parameter(s) is/are optional.

## Getting hold of the selection

The way to obtain a selection object from Rangy is:

```
var sel = rangy.getSelection();
```

Note that in IE < 9 (and in IE 9 when `rangy.config.preferTextRange` is set to `true`), using this and other selection methods may break the browser's built-in undo stack.

You can also obtain a selection for a document within another window, frame or iframe, a common requirement for browser-based rich text editors:

```
var iframe = document.getElementById("foo");
var sel = rangy.getSelection(iframe);
```

To move the selection to encompass the contents of an element:

```
var el = document.getElementById("foo");
var range = rangy.createRange();
range.selectNodeContents(el);
var sel = rangy.getSelection();
sel.setSingleRange(range);
```

`setSingleRange()` is a convenience method in Rangy not present in browser selection objects. It removes the need to call `sel.removeAllRanges()` before selecting a range.

## Working with a Rangy selection

The most important thing to understand is that **a Rangy selection does not automatically keep in sync with the underlying browser selection**. Any method call on the Rangy selection will make all the necessary updates on the browser selection and the Rangy selection, but for any other change to the selection you need to tell the Rangy selection to update itself by using its `refresh()` method.

Typical scenarios where the `refresh()` method is needed:

 * Calling a method on the browser's own selection object, bypassing Rangy;
 * Keeping a reference to a Rangy selection lying around between events so that the user will have had the opportunity to change the selection since the Rangy selection was last used.

Another point to note is that browsers differ in whether the selection keeps in sync with changes to its underlying range. The specification mandates that it should but only Mozilla does this. Prior to version 1.3, Rangy's selection mirrors the inconsistent browser behaviour; from 1.3, the range added to the selection is always cloned, meaning the range and selection do not stay in sync. This is contrary to the specification but achieves consistency without significant loss of functionality.

Rangy selections do not provide all the functionality that native `Selection` objects have in some browsers (in particular, there is no implementation of the useful [extend()](https://developer.mozilla.org/en/DOM/Selection/extend) method, because it is simply impossible to replicate in any version of IE). You can access the browser's selection object through the Rangy selection's `nativeSelection` property.

## Control selections in Internet Explorer

Internet Explorer supports two different kind of selections. The first, familiar kind is of type "Text" and consists of a portion of content in the page highlighted with a solid background colour, usually dark blue. The other kind is of type "Control" and represents the selection when one or more elements such as images or tables are selected for moving or resizing. Each selected element has visible resize handles around the edge. Multiple elements may not be selected simultaneously by default, but can be with the following code:

```
document.execCommand("MultipleSelection", null, true);
```

Calling the [createRange()](http://msdn.microsoft.com/en-us/library/ms536394%28v=VS.85%29.aspx) method of the browser's native selection object will provide a [ControlRange](http://msdn.microsoft.com/en-us/library/ms537447%28VS.85%29.aspx ControlRange) object rather than a [TextRange](http://msdn.microsoft.com/en-us/library/ms535872%28VS.85%29.aspx) that you would get with a "Text"-type selection. A ControlRange contains a collection of elements and provides methods to add and remove elements. From my experiments, it appears that only elements that have [layout](http://www.satzansatz.de/cssd/onhavinglayout.html) (also see [MSDN](http://msdn.microsoft.com/en-us/library/bb250481%28VS.85%29.aspx MSDN)) may be added to a ControlRange.

Rangy's Selection object fully supports this kind of selection, in the following ways:

  * For a control selection, each selected element is represented in the Rangy selection as a Range that encompasses the element.
  * For a control selection, Rangy's Selection object allows multiple Ranges when the native browser selection allows multiple elements within a control selection. This means that `addRange()` will allow you to add a Range that exactly encompasses a single element. If the Range does not contain exactly one element or the element is not valid within a control selection (i.e. it does not have layout), an error is thrown.
  * `setRanges()` automatically tries to create a control selection in Internet Explorer if multiple ranges are specified. As with `addRange()`, each Range must exactly encompass a single element.
  * If the selection is a control selection, `removeRange()` tries to remove an element from the selection corresponding to the element contained within the specified Range. As with `addRange()`, each Range must exactly encompass a single element.

## Properties

All selection properties are read-only. However, Rangy does not enforce this so it is possible to change properties directly. Do not do this; doing so is very unlikely to achieve what you want.

----

#### `nativeSelection`

*Non-standard*

Returns a reference to the underlying native browser `Selection` object.

----

#### `rangeCount`

[Selection API](http://www.w3.org/TR/selection-api/#widl-Selection-rangeCount), [MDN](https://developer.mozilla.org/en-US/docs/Web/API/Selection/rangeCount)

Returns the number of Ranges in the selection. No major browser other than Firefox currently allows multiple selections, so this property will only return 0 or 1 in non-Firefox browsers.

----

#### `isCollapsed`

[Selection API](http://www.w3.org/TR/selection-api/#widl-Selection-isCollapsed), [MDN](https://developer.mozilla.org/en-US/docs/Web/API/Selection/isCollapsed)

Returns a Boolean value indicating whether the selection's start and end points are at the same position.

----

#### `anchorNode`, `anchorOffset`, `focusNode`, `focusOffset`

[Selection API](http://www.w3.org/TR/selection-api/#widl-Selection-anchorNode), [MDN](https://developer.mozilla.org/en-US/docs/Web/API/Selection/anchorNode)

The `anchorNode` and `anchorOffset` properties represent a position within the document called the selection's _anchor_ while the `focusNode` and `focusOffset` properties represent a position within the document called the selection's _focus_. The anchor is the point within the document at which the user started selecting the final Range within the selection while the focus is the point at which the user stopped selecting. Note that the anchor may be located later in the document than the focus (in which case the selection is "backwards").

## Methods

----

#### `addRange(Range range[, String direction])`

[Selection API](http://www.w3.org/TR/selection-api/#widl-Selection-addRange-void-Range-range), [MDN](https://developer.mozilla.org/en-US/docs/Web/API/Selection/addRange)

Adds a range to the selection. This may be a Rangy range or a native browser `Range`; in the latter case, the range that is stored in the selection is a Rangy range that wraps the original range.

This method has an optional `direction` parameter that native browser `Selection` objects do not have. This parameter specifies whether the range is to be selected "backwards" (i.e. from end to start). Note that this parameter has no effect in Internet Explorer, which lacks the ability to programmatically set the direction of a selection.

`direction` may be any of the strings "forward", "forwards", "backward" or "backwards" or a Boolean (in which case `true` corresponds to "backwards").

In some browsers (particularly WebKit-based browsers), the range that is stored within the selection and returned by the `getRangeAt()` method may have different properties to the range supplied to `addRange()`. To deal with this, by default Rangy checks whether the range that ends up in the native browser selection object is different to the range supplied to `addRange()` and if so, updates its own range. However, you can disable this check and Rangy's selection will instead store the untouched original range. You can do this as follows:

```
rangy.config.checkSelectionRanges = false;
```

For more discussion on this, see the notes for `getRangeAt()` below.

Note that it is possible for this method to fail to add a range and clear the selection without throwing an error. This happens in Opera when you try to add a collapsed range that is contained within a non-editable element.

In Internet Explorer, if the native browser selection is a control selection, this method will attempt to add the element within the specified range to the control selection. See the [Control selections in Internet Explorer section](#Control_selections_in_Internet_Explorer) for more information.

----

#### `getRangeAt(Number index)`

[Selection API](http://www.w3.org/TR/selection-api/#widl-Selection-getRangeAt-Range-unsigned-long-index), [MDN](https://developer.mozilla.org/en-US/docs/Web/API/Selection/getRangeAt)

Returns a range for the index provided. The ranges in the selection are indexed in order of when they were added to the selection.

Some notes about using this method:

 * The range returned by this method may or may not be the self-same range object that was originally added to the selection;
 * The range returned by this method is not guaranteed to have the same properties as the range that was originally added to the selection.

Browsers are divided on the first point and Rangy does not try to compensate. On the second point, browsers vary significantly in their behaviour. In WebKit, for example, a native `Selection` object will often return a range that has different properties from the original range when `getRangeAt` is called. Since the range returned by `getRangeAt` reflects the reality of the browser's selection, Rangy does not attempt to compensate for this. In IE <= 8, the conversion of `TextRange` to Rangy range is fairly sophisticated but since `TextRange`s are not expressed in terms of DOM nodes and offsets, there is inevitable ambiguity over the precise location of a Range boundary that does not fall within the middle of a text node.

----

#### `isBackwards()`

*Non-standard*

Returns a Boolean indicating whether the selection is backwards (i.e. the selection's focus is earlier in the document than the anchor).

----

#### `removeAllRanges()`

[Selection API](http://www.w3.org/TR/selection-api/#widl-Selection-removeAllRanges-void), [MDN](https://developer.mozilla.org/en-US/docs/Web/API/Selection/removeAllRanges)

Removes all ranges from the selection, thus clearing it.

----

#### `removeRange(Range range)`

[Selection API](http://www.w3.org/TR/selection-api/#widl-Selection-removeRange-void-Range-range), [MDN](https://developer.mozilla.org/en-US/docs/Web/API/Selection/removeRange)

Removes the specified range from the selection. See the note above for `getRangeAt()`: since there is no guarantee that a range object you add to the selection will be stored unchanged within the selection, this method is unreliable and its use is not recommended.

In Internet Explorer, if the native browser selection is a control selection, this method will attempt to remove the element within the specified range from the control selection. See the [Control selections in Internet Explorer section](#Control_selections_in_Internet_Explorer) for more information.

----

#### `refresh([Boolean checkForChanges])`

*Non-standard*

Synchronises the selection with the underlying native browser selection. Call this method after you've manipulated the native selection object or if the user has had the chance to change the selection.

If `checkForChanges` is specified and `true`, this method returns a Boolean indicating whether or not the refresh changed the selection (`true` if the selection was changed, `false` otherwise).

----

#### `collapse(Node node, Number offset)`

[Selection API](http://www.w3.org/TR/selection-api/#widl-Selection-collapse-void-Node-node-unsigned-long-offset), [MDN](https://developer.mozilla.org/en-US/docs/Web/API/Selection/collapse)

Collapses the selection to a point specified by the node and offset.

##### Example
To collapse the selection to the start of the body:

```
var sel = rangy.getSelection();
sel.collapse(document.body, 0);
```

----

#### `collapseToStart()`

[Selection API](http://www.w3.org/TR/selection-api/#widl-Selection-collapseToStart-void), [MDN](https://developer.mozilla.org/en-US/docs/Web/API/Selection/collapseToStart)

Collapses the selection to the start of the first range in the selection.

----

#### `collapseToEnd()`

[Selection API](http://www.w3.org/TR/selection-api/#widl-Selection-collapseToEnd-void), [MDN](https://developer.mozilla.org/en-US/docs/Web/API/Selection/collapseToEnd)

Collapses the selection to the end of the last range in the selection.

----

#### `selectAllChildren(Node node)`

[Selection API](http://www.w3.org/TR/selection-api/#widl-Selection-selectAllChildren-void-Node-node), [MDN](https://developer.mozilla.org/en-US/docs/Web/API/Selection/selectAllChildren)

Clears the selection and selects the contents of the specified node.

----

#### `deleteFromDocument()`

[Selection API](http://www.w3.org/TR/selection-api/#widl-Selection-deleteFromDocument-void), [MDN](https://developer.mozilla.org/en-US/docs/Web/API/Selection/deleteFromDocument)

Deletes the contents of the selection from the document.

----

#### `toString()`

[Selection API](http://www.w3.org/TR/selection-api/#widl-Selection-DOMString-stringifier), [MDN](https://developer.mozilla.org/en-US/docs/Web/API/Selection/toString)

Returns the text within the selection.

Note that this method is not currently specified by the [W3C HTML Editing specification](http://dvcs.w3.org/hg/editing/raw-file/tip/editing.html#dom-selection-stringifier). Rangy, like IE 9, follows an earlier version of the spec, which mandated that this method should return a concatenation of the the result of calling `toString()` on each Range in the selection. However, this differs from many browser implementations, notably Mozilla and WebKit, which have more complicated (and different) implementations that attempt to return something closer to the text visible to the user.

----

#### `toHtml()`

*Non-standard*

##### From v1.2

Returns a string containing an HTML representation of the selection.

----

#### `getAllRanges()`

*Non-standard*

Returns an array containing all the ranges within the selection.

----

#### `getNativeTextRange()`

*Non-standard*

##### From v1.3

Returns a native `TextRange` object representing the selection in browsers that support `TextRange` (which means Internet Explorer).

This is particularly designed for IE 11 which drops support for `document.selection` but retains support for `TextRange`, which has useful methods not supported by DOM `Range`.

----

#### `setSingleRange(Range range[, String direction])`

*Non-standard*

Replaces the current selection with the specified range. Please see `addRange()` above for the meaning of `direction`.

----

#### `setRanges(Array ranges)`

*Non-standard*

Replaces the current selection with the specified ranges. In Internet Explorer, if multiple Ranges are specified, this method will attempt to create a control selection. See the [Control selections in Internet Explorer section](#Control_selections_in_Internet_Explorer) for more information.

----

#### `containsNode(Node node, Boolean partial)`

[Selection API](http://www.w3.org/TR/selection-api/#widl-Selection-containsNode-boolean-Node-node-boolean-allowPartialContainment), [MDN](https://developer.mozilla.org/en-US/docs/Web/API/Selection/containsNode)

Returns a Boolean indicating whether the selection contains (or partially contains, if `partial` is `true`) the specified node.

----

#### `getBookmark([Node containerNode])`

*Non-standard*

##### From v1.3

Returns an object containing bookmarks generated by calling `getBookmark()` on each selected range using character offsets relative to the text content of `containerNode`. If `containerNode` is not specified, the current document's `<body>` element is used.

This method used with `moveToBookmark()` is useful for saving and restoring a range when radical changes are being made to an element's DOM that don't affect the element's text. One example is changing the element's `innerHTML` property.

----

#### `moveToBookmark(Object bookmark)`

*Non-standard*

##### From v1.3

Restores the selection to a previously saved bookmark returned by `getBookmark()`.

----

#### `saveRanges()`

*Non-standard*

##### From v1.3

Returns an object containing an array containing copies of the selection's ranges and a direction (except in Internet Explorer <= 8). This can be passed into `restoreRanges()` later to restore a previously saved selection state.

----

#### `restoreRanges(saved)`

*Non-standard*

##### From v1.3

Restores the selection to a previous state represented by `saved`. `saved` should previously have been returned by a call to `saveRanges()`. Note that it is impossible in all versions of Internet Explorer to set the direction of the selection programmatically so the selection is always restored forwards in those browsers.

Note that this may not work if there have been changes to the DOM since calling `saveRanges()`. For such cases, Rangy provides other options:

- basic character offset-based using `getBookmark()` and `moveToBookmark()`
- more sophisticated character offset-based using the [[TextRange module|Text Range Module]]
- temporary mark elements-based using the [[Selection save and restore module|Selection Save Restore Module]]

----

#### `detach()`

*Non-standard*

Detaches the selection from its associated `Window` object and renders it unusable. You should call this method when you no longer need the selection.

----

#### `inspect()`

*Non-standard*

Returns a readable string representation of the selection. For example,

```
var sel = rangy.getSelection();
sel.collapse(document.body, 0);
alert(sel.inspect())            
```

... alerts `[WrappedSelection([WrappedRange(<BODY>:0, <BODY>:0)])]`

## Prototype

----

From version 1.2, Rangy exposes a prototype Selection object as `rangy.selectionPrototype` which may be used to add custom extensions to all Rangy Selection objects.

For example, the following adds a convenient `selectNode()` method to all selections:

```
rangy.selectionPrototype.selectNode = function(node) {
    var range = rangy.createRange();
    range.selectNode(node);
    this.setSingleRange(range);
};
```