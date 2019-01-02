[[Documentation home|Home]] 

# Range objects in Rangy

## Contents

  * [Introduction](#introduction)
  * [Working with a Rangy Range](#working-with-a-rangy-range)
  * [Creating a Range](#creating-a-range)
  * [API: Properties](#properties)
  * [API: Methods](#methods)

## Introduction

Rangy contains a full JavaScript implementation of the methods and properties of [DOM4 Range](http://www.w3.org/TR/dom/#introduction-to-dom-ranges), forming the basis of the support for Internet Explorer 6-8 browsers, which have no Range implementation. In other browsers, Rangy provides a wrapper for native Range objects, in general delegating functionality to the faster built-in browser implementation. However, there are some bugs in browser implementations of Range, which Rangy detects and fixes by providing its own methods in place of the broken ones.

Rangy also provides a number of extra methods on it Range objects, including all of Mozilla's non-standard methods and number of its own extra methods.

You can see a live example of Rangy's range objects in action [here](http://rangy.googlecode.com/svn/trunk/demos/core.html).

## Working with a Rangy Range

While Rangy's Range objects implement every property and method of DOM4 Range, there are some key differences with the specification (and therefore native browser implementations). These come as a consequence of Rangy's goal of providing the same functionality in all supported browsers.

1. **Range objects do not update when the DOM is changed (except when it is a range method doing the changing).**
2. None of the properties of a range throw an exception when assigned to.
3. Properties of a detached range throw no exceptions when accessed.

None of these should pose a serious problem or affect normal usage, provided you follow some guidelines:

1. Create a new range whenever there is possibility that some user action or other code has changed the document
2. Do not try to assign to range properties (`startContainer`, `endContainer`, `startOffset`, `endOffset`, `commonAncestorContainer` and `collapsed`). There will be no error message and things will probably subsequently go wrong.

## Creating a range

The usual way of creating a range in Rangy is to use the `rangy` object's `createRange()` method. Note that in IE < 9 (and in IE >= 9 when `rangy.config.preferTextRange` is set to `true`), using this method will break the browser's built-in undo stack.

```
var range = rangy.createRange();
```

You can also create a range within a document in another window, frame or iframe, a common requirement for a rich text editor:

```
var iframe = document.getElementById("foo");
var range = rangy.createRange(iframe);
```

There are two related methods for creating a range:

----

#### `rangy.createNativeRange([Document doc])`

*Non-standard*

In IE 6-8, this method creates and returns a `TextRange` object for the specified document. In other browsers, it creates and returns a native browser `Range` object for the document specified.

The `doc` parameter is optional and defaults to the current document. From version 1.3, a Window object or iframe element may be used instead.

----

#### `rangy.createRangyRange([Document doc])`

*Non-standard*

This method creates a pure JavaScript Range object that does not wrap a native browser Range. This has all the same properties and methods as a range created with `rangy.createRange()` and can be used in exactly the same way. The advantage of this kind of range is that it's guaranteed to behave the same in all browsers, but at the cost of inferior performance.

The `doc` parameter is optional and defaults to the current document. From version 1.3, a Window object or iframe element may be used instead.

## Properties

All range properties are read-only. However, Rangy does not enforce this so it is possible to change properties directly. Do not do this; doing so is very unlikely to achieve what you want.

----

#### `startContainer`

[DOM 2](http://www.w3.org/TR/DOM-Level-2-Traversal-Range/ranges.html#Level-2-Range-attr-startParent), [DOM 4](http://www.w3.org/TR/dom/#dom-range-startcontainer), [MDN](https://developer.mozilla.org/en-US/docs/Web/API/range/startContainer)

Returns the node within which the range starts.

----

#### `startOffset`

[DOM 2](http://www.w3.org/TR/DOM-Level-2-Traversal-Range/ranges.html#Level-2-Range-attr-startOffset), [DOM 4](http://www.w3.org/TR/dom/#dom-range-startoffset), [MDN](https://developer.mozilla.org/en-US/docs/Web/API/range/startOffset)

Returns number representing the offset of the start of the range within its `startContainer`. In the case of character-based nodes (text nodes, CDATA nodes and comment nodes), this is the number of characters before the start of the range. For other nodes, it's the number of child nodes of `startContainer` preceding the start of the range.

----

#### `endContainer`

[DOM 2](http://www.w3.org/TR/DOM-Level-2-Traversal-Range/ranges.html#Level-2-Range-attr-endParent), [DOM 4](http://www.w3.org/TR/dom/#dom-range-endcontainer), [MDN](https://developer.mozilla.org/en-US/docs/Web/API/range/endContainer)

Returns the node within which the range ends.

----

#### `endOffset`

[DOM 2](http://www.w3.org/TR/DOM-Level-2-Traversal-Range/ranges.html#Level-2-Range-attr-endOffset), [DOM 4](http://www.w3.org/TR/dom/#dom-range-endoffset), [MDN](https://developer.mozilla.org/en-US/docs/Web/API/range/endOffset)

Returns number representing the offset of the end of the range within its `endContainer`. In the case of character-based nodes (text nodes, CDATA nodes and comment nodes), this is the number of characters before the end of the range. For other nodes, it's the number of child nodes of `endContainer` preceding the end of the range.

----

#### `commonAncestorContainer`

[DOM 2](http://www.w3.org/TR/DOM-Level-2-Traversal-Range/ranges.html#Level-2-Range-attr-commonParent), [DOM 4](http://www.w3.org/TR/dom/#dom-range-commonancestorcontainer), [MDN](https://developer.mozilla.org/en-US/docs/Web/API/range/commonAncestorContainer)

Returns the deepest node within the document that contains the whole range.

----

#### `collapsed`

Returns a Boolean indicating whether the range is collapsed (i.e. the start boundary is identical to the end boundary).

[DOM 2](http://www.w3.org/TR/DOM-Level-2-Traversal-Range/ranges.html#Level-2-Range-attr-collapsed), [DOM 4](http://www.w3.org/TR/dom/#dom-range-collapsed), [MDN](https://developer.mozilla.org/en-US/docs/Web/API/range/collapsed)

## Methods

----

#### `setStart(Node node, Number offset)`

[DOM 2](http://www.w3.org/TR/DOM-Level-2-Traversal-Range/ranges.html#Level2-Range-method-setStart), [DOM 4](http://www.w3.org/TR/dom/#dom-range-setstart), [MDN](https://developer.mozilla.org/en-US/docs/Web/API/range/setStart)

Sets the start boundary of the range.

----

#### `setStartBefore(Node node)`

[DOM 2](http://www.w3.org/TR/DOM-Level-2-Traversal-Range/ranges.html#Level2-Range-method-setStartBefore), [DOM 4](http://www.w3.org/TR/dom/#dom-range-setstartbefore), [MDN](https://developer.mozilla.org/en-US/docs/Web/API/range/setStartBefore)

Sets the start boundary of the range to be immediately before the specified node.

----

#### `setStartAfter(Node node)`

[DOM 2](http://www.w3.org/TR/DOM-Level-2-Traversal-Range/ranges.html#Level2-Range-method-setStartAfter), [DOM 4](http://www.w3.org/TR/dom/#dom-range-setstartafter), [MDN](https://developer.mozilla.org/en-US/docs/Web/API/range/setStartAfter)

Sets the start boundary of the range to be immediately after the specified node.

----

#### `setEnd(Node node, Number offset)`

[DOM 2](http://www.w3.org/TR/DOM-Level-2-Traversal-Range/ranges.html#Level2-Range-method-setEnd), [DOM 4](http://www.w3.org/TR/dom/#dom-range-setend), [MDN](https://developer.mozilla.org/en-US/docs/Web/API/range/setEnd)

Sets the end boundary of the range.

----

#### `setEndBefore(Node node)`

[DOM 2](http://www.w3.org/TR/DOM-Level-2-Traversal-Range/ranges.html#Level2-Range-method-setEndBefore), [DOM 4](http://www.w3.org/TR/dom/#dom-range-setendbefore), [MDN](https://developer.mozilla.org/en-US/docs/Web/API/range/setEndBefore)

Sets the end boundary of the range to be immediately before the specified node.

----

#### `setEndAfter(Node node)`

[DOM 2](http://www.w3.org/TR/DOM-Level-2-Traversal-Range/ranges.html#Level2-Range-method-setEndAfter), [DOM 4](http://www.w3.org/TR/dom/#dom-range-setendafter), [MDN](https://developer.mozilla.org/en-US/docs/Web/API/range/setEndAfter)

Sets the end boundary of the range to be immediately after the specified node.

----

#### `setStartAndEnd(Node startNode, Number startOffset[, Node endNode][, Number endOffset])`

*Non-standard*

##### From v1.3

Sets the start and end boundaries of the range.

The number of arguments is variable. The forms are:

#### `setStartAndEnd(Node node, Number offset)`

Collapses the range to a single point represented by the node and offset. This is identical to `collapseTo(node, offset)`.

#### `setStartAndEnd(Node node, Number startOffset, Number endOffset)`

Sets the range to start and end within `node`, spanning the range between `startOffset` and `endOffset` within `node`.

#### `setStartAndEnd(Node startNode, Number startOffset, Node endNode, Number endOffset)`

Sets the range to start at offset `startOffset` within node `startNode` and end at offset `endOffset` within node `endNode`. This is identical to `setStart(startNode, startOffset)` followed by `setEnd(endNode, endOffset)`.

----

#### `selectNode(Node node)`

[DOM 2](http://www.w3.org/TR/DOM-Level-2-Traversal-Range/ranges.html#Level2-Range-method-selectNode), [DOM 4](http://www.w3.org/TR/dom/#dom-range-selectnode), [MDN](https://developer.mozilla.org/en-US/docs/Web/API/range/selectNode)

Moves the range boundary points to encompass the specified node. `startContainer` and `endContainer` are set to the node's `parentNode`; defining `i` to be the index of `node` within its parent, `startOffset` is set to `i` and `endOffset` is set to `i + 1`.

For example, in the HTML below:

```
<p>Watford's all-time top scorer is <b id="luther">Luther Blissett</b> with 186 goals.</p>
```

... you could create a range that encompassed the `<b>` element:

```
var range = rangy.createRange();
var b = document.getElementById("luther");
range.selectNode(b);
```

`startContainer` and `endContainer` are both references to the `<p>` element. `startOffset` is 1 and `endOffset` is 2.

----

#### `selectNodeContents(Node node)`

[DOM 2](http://www.w3.org/TR/DOM-Level-2-Traversal-Range/ranges.html#Level2-Range-method-selectNodeContents), [DOM 4](http://www.w3.org/TR/dom/#dom-range-selectnodecontents), [MDN](https://developer.mozilla.org/en-US/docs/Web/API/range/selectNodeContents)

Moves the range to encompass the contents of the specified node.

In all cases, `startContainer` and `endContainer` are set to `node`.

In the case of a node that can have child nodes (such as an element), the start boundary is placed immediately before the first child of `node` and the end boundary immediately after the last child of `node`, meaning that `startOffset` is set to 0 and `endOffset` to `node.childNodes.length`.

For a node that cannot have child nodes (such as a text node), the start boundary is placed at the start of the node's text and the end boundary at the end of the node's text, meaning that `startOffset` is set to 0 and `endOffset` to `node.length`.

For example, in the HTML below:

```
<p>Watford's all-time top scorer is <b id="luther">Luther Blissett</b> with 186 goals.</p>
```

... you could create a range that encompassed the contents of the `<b>` element:

```
var range = rangy.createRange();
var b = document.getElementById("luther");
range.selectNodeContents(b);
```

`startContainer` and `endContainer` are both references to the `<b>` element. `startOffset` is 0 and `endOffset` is 1.

Calling `selectNodeContents()` with the text node inside the `<b>` element:

```
range.selectNodeContents(b.firstChild);
```

`startContainer` and `endContainer` are both references to the text node. `startOffset` is 0 and `endOffset` is 15.

----

#### `collapse(Boolean toStart)`

[DOM 2](http://www.w3.org/TR/DOM-Level-2-Traversal-Range/ranges.html#Level2-Range-method-collapse), [DOM 4](http://www.w3.org/TR/dom/#dom-range-collapse), [MDN](https://developer.mozilla.org/en-US/docs/Web/API/range/collapse)

Collapses the range to either its the start or end boundary, depending on `toStart`.

----

#### `compareBoundaryPoints(Number comparisonType, Range range)`

[DOM 2](http://www.w3.org/TR/DOM-Level-2-Traversal-Range/ranges.html#Level2-Range-method-compareBoundaryPoints), [DOM 4](http://www.w3.org/TR/dom/#dom-range-compareboundarypoints), [MDN](https://developer.mozilla.org/en-US/docs/Web/API/range/compareBoundaryPoints)

Compares a boundary of the current range with a boundary of the specified range and returns -1, 0 or 1 if the current range's boundary is respectively before, equal to or after `range`'s boundary. The boundaries that are compared are determined by `comparisonType` and should be one of the following constants (provided both as properties of every range and also as properties of the `Range` constructor):

 * `START_TO_START`: compares the start boundary of both ranges
 * `START_TO_END`: compares the end boundary of the current range to the start boundary of `range`
 * `END_TO_START`: compares the start boundary of the current range to the end boundary of `range`
 * `END_TO_END`: compares the end boundary of both ranges

----

#### `insertNode(Node node)`

[DOM 2](http://www.w3.org/TR/DOM-Level-2-Traversal-Range/ranges.html#Level2-Range-method-insertNode), [DOM 4](http://www.w3.org/TR/dom/#dom-range-insertnode), [MDN](https://developer.mozilla.org/en-US/docs/Web/API/range/insertNode)

Inserts the specified node at the start of the range and moves the start boundary of the range to be immediately before the inserted node.

----

#### `cloneContents()`

[DOM 2](http://www.w3.org/TR/DOM-Level-2-Traversal-Range/ranges.html#Level2-Range-method-cloneContents), [DOM 4](http://www.w3.org/TR/dom/#dom-range-clonecontents), [MDN](https://developer.mozilla.org/en-US/docs/Web/API/range/cloneContents)

Returns a `DocumentFragment` containing a copy of the content contained within the range.

----

#### `extractContents()`

[DOM 2](http://www.w3.org/TR/DOM-Level-2-Traversal-Range/ranges.html#Level2-Range-method-extractContents), [DOM 4](http://www.w3.org/TR/dom/#dom-range-extractcontents), [MDN](https://developer.mozilla.org/en-US/docs/Web/API/range/extractContents)

Moves the content contained within the range into a `DocumentFragment` and returns the fragment.

----

#### `deleteContents()`

[DOM 2](http://www.w3.org/TR/DOM-Level-2-Traversal-Range/ranges.html#Level2-Range-method-deleteContents), [DOM 4](http://www.w3.org/TR/dom/#dom-range-deletecontents), [MDN](https://developer.mozilla.org/en-US/docs/Web/API/range/deleteContents)

Deletes the content contained within the range from the document.

----

#### `canSurroundContents()`

*Non-standard*

Returns a Boolean indicating whether or not it is possible to surround the contents of the range inside a DOM node.

----

#### `surroundContents(Node node)`

[DOM 2](http://www.w3.org/TR/DOM-Level-2-Traversal-Range/ranges.html#Level2-Range-method-surroundContents), [DOM 4](http://www.w3.org/TR/dom/#dom-range-surroundcontents), [MDN](https://developer.mozilla.org/en-US/docs/Web/API/range/surroundContents)

Moves the content contained within the range into the specified node, which is inserted into the document at the position where the range had been.

Note that this method will throw an error if the range _partially selects_ a non-text node. A node is said to be partially selected by a Range if it contains exactly one boundary point of the Range.

----

#### `cloneRange()`

[DOM 2](http://www.w3.org/TR/DOM-Level-2-Traversal-Range/ranges.html#Level2-Range-method-cloneRange), [DOM 4](http://www.w3.org/TR/dom/#dom-range-clonerange), [MDN](https://developer.mozilla.org/en-US/docs/Web/API/range/cloneRange)

Returns a new range with identical boundary points to the current range.

----

#### `isValid()`

*Non-standard*

##### From v1.2

Returns a Boolean that is `true` if the range start and end boundaries are valid, i.e. the boundary container nodes are in the same node tree and the boundary offsets are valid for the boundary nodes.

----

#### `toString()`

[DOM 2](http://www.w3.org/TR/DOM-Level-2-Traversal-Range/ranges.html#Level2-Range-method-toString), [DOM 4](http://www.w3.org/TR/dom/#dom-range-stringifier), [MDN](https://developer.mozilla.org/en-US/docs/Web/API/range/toString)

Returns the text contained within the range.

----

#### `toHtml()`

*Non-standard*

##### From v1.2

Returns a string containing an HTML representation of the range.

----

#### `compareNode(Node node)`

*Non-standard, Mozilla ([MDN](https://developer.mozilla.org/en-US/docs/Web/API/range/compareNode))*

Returns a constant representing whether the node is before, after, inside, or surrounding the range. These constants are exposed as properties of every range object and are:

 * NODE_BEFORE: the node starts before the range
 * NODE_AFTER: the node ends after the range
 * NODE_BEFORE_AND_AFTER: the node starts before and ends after the range 
 * NODE_INSIDE: the node is completely contained within the range

----

#### `comparePoint(Node node, Number offset)`

[DOM 4](http://www.w3.org/TR/dom/#dom-range-comparepoint), [MDN](https://developer.mozilla.org/en-US/docs/Web/API/range/comparePoint)

Returns -1, 0, or 1 depending on whether the point represented by the node and offset is before, equal to or after the range.

----

#### `intersectsOrTouchesRange(Range range)`

*Non-standard*

#### From v1.3

Returns a Boolean indicating whether the current range intersects or touches (i.e. shares a boundary with, start-to-end or end-to-start) the specified range.

----

#### `intersectsRange(Range range)`

*Non-standard*

Returns a Boolean indicating whether the current range intersects the specified range. Sharing a boundary start-to-end or end-to-start does not count as intersection.

----

#### `intersectsNode(Node node[, touchingIsIntersecting])`

[DOM 4](http://www.w3.org/TR/dom/#dom-range-intersectsnode), [MDN](https://developer.mozilla.org/en-US/docs/Web/API/range/intersectsNode)

Returns a Boolean indicating whether the current range intersects the specified node. `touchingIsIntersecting` is a non-standard extension and controls whether a node that borders the current range is counted as intersecting with it. The default in Rangy is `false`, as per the DOM4 specification. Despite this method's adoption in DOM4, browsers still disagree on this point: WebKit says yes (this deviating from the spec), Mozilla says no.

----

#### `intersection(Range range)`

*Non-standard*

##### From v1.1

Returns a Range representing the portion of the document that is contained within both the current range and the specified range or `null` if there is no overlap.

----

#### `union(Range range)`

*Non-standard*

##### From v1.2

Providing `range` overlaps or touches the current range, returns the smallest possible Range containing the whole of both ranges, or `null` otherwise.

----

#### `isPointInRange(Node node, Number offset)`

[DOM 4](http://www.w3.org/TR/dom/#dom-range-ispointinrange), [MDN](https://developer.mozilla.org/en-US/docs/Web/API/range/isPointInRange)


Returns a Boolean indicating whether the position represented by the specified node and offset is contained within the current range. The boundary positions of the range are considered to be within the range for the purposes of this method.

----

#### `createContextualFragment(String html)`

[DOM Parsing and Serialization](https://dvcs.w3.org/hg/innerhtml/raw-file/tip/index.html#extensions-to-the-range-interface), [MDN](https://developer.mozilla.org/en-US/docs/Web/API/range/createContextualFragment)


Returns a `DocumentFragment` containing the content represented by `html`.

----

#### `containsNode(Node node, Boolean partial)`

*Non-standard*

Returns a Boolean indicating whether the range contains (or partially contains, if `partial` is `true`) the specified node.

----

#### `containsNodeContents(Node node)`

*Non-standard*

Returns a Boolean indicating whether the range contains the specified node's contents.

----

#### `containsNodeText(Node node)`

*Non-standard*

##### From v1.2

Returns a Boolean indicating whether the range contains all of the text (within text nodes) contained within `node`. This is to provide an intuitive means of checking whether a range "contains" a node if you consider the range as just in terms of the text it contains without having to worry about niggly details about range boundaries.

----

#### `containsRange(Range range)`

*Non-standard*

#### From v1.2

Returns a Boolean indicating whether the current range completely contains `range`.

----

#### `splitBoundaries()`

*Non-standard*

Separates the contents of the range into discrete nodes which can subsequently be modified without affecting content outside the range. In practice, this means that a character-based node such as a text node that is partially selected by the range is split at the boundary point of the range.

----

#### `normalizeBoundaries()`

*Non-standard*

Reverses the effects of a call to `splitBoundaries()` by gluing text nodes at each end of the range back together with any surrounding text nodes.

----

#### `collapseToPoint(Node node, Number offset)`

*Non-standard*

Collapses the range to a single point represented by the node and offset.

----

#### `collapseBefore(Node node)`

*Non-standard*

Collapses the range to a single point immediately before the specified node.

----

#### `collapseAfter(Node node)`

*Non-standard*

Collapses the range to a single point immediately after the specified node.

----

#### `getNodes([Array nodeTypes[, Function filter]])`

*Non-standard*

Returns an array containing all the nodes partially or wholly contained within the range that are of the types specified and satisfy the specified filter. For a collapsed range, this method always returns an empty array. If `nodeTypes` is falsy then nodes of all node types present within the range are returned; if no filter is supplied, all nodes of the specified types are returned.

If supplied, `filter` should be a function that receives a single `Node` parameter and returns a Boolean value indicating whether the node supplied to the function should be included in the array of nodes returned by `getNodes()`.

For example, the following will obtain all the text nodes within the document body that contain the word "foo", case insensitively:

```
var range = rangy.createRange();
range.selectNode(document.body);
var textNodes = range.getNodes([3], function(node) {
    return /\bfoo\b/i.test(node.data);
});
```

----

#### `getBookmark([Node containerNode])`

*Non-standard*

##### From v1.3

Returns an object with properties `start` and `end` representing the start and end of the range as character offsets relative to the text content of `containerNode`. If `containerNode` is not specified, the current document's `<body>` element is used. The returned object also has a `containerNode` property.

This method used with `moveToBookmark()` is useful for saving and restoring a range when radical changes are being made to an element's DOM that don't affect the element's text. One example is changing the element's `innerHTML` property.

----

#### `moveToBookmark(Object bookmark)`

*Non-standard*

##### From v1.3

Moves a range to a bookmark object such as a previously saved bookmark returned by `getBookmark()`. You can create your own bookmark object with properties `containerNode`, `start` and `end` as described above.

----

#### `getDocument()`

*Non-standard*

##### From v1.2

Returns the `Document` element containing the range.

----

#### `detach()`

[DOM 2](http://www.w3.org/TR/DOM-Level-2-Traversal-Range/ranges.html#Level2-Range-method-detach), [DOM 4](http://www.w3.org/TR/dom/#dom-range-detach), [MDN](https://developer.mozilla.org/en-US/docs/Web/API/range/detach)

From version 1.3, in line with the DOM4 specification, this method does nothing.

----

#### `inspect()`

*Non-standard*

Returns a readable string representation of the range. For example,

```
var range = rangy.createRange();
range.collapseToPoint(document.body, 0);
alert(range.inspect());
```

... alerts `[WrappedRange(<BODY>:0, <BODY>:0)]`

----

#### `equals(Range range)`

*Non-standard*

##### From v1.1

Returns a Boolean indicating whether the current range's boundaries are identical to those of the specified range (which may be a Rangy Range or a native DOM Range).

----

#### `refresh()`

*Non-standard*

##### From v1.1

##### Not available on pure Rangy Ranges created by `rangy.createRangyRange()`

Synchronizes the range with the underlying native range. Behaviour may vary between browsers; in particular, IE's TextRange objects behave differently under DOM mutation to DOM Range objects found in other browsers. For this reason, use of this method is not generally recommended.

----

#### `select()`

*Non-standard*

##### From v1.3

Replaces the current selection with this range. Equivalent to `rangy.getSelection().setSingleRange(range)`.

This is the only method of Range that directly affects the selection.

## Prototype

----

From version 1.2, Rangy exposes a prototype Range object as `rangy.rangePrototype` which may be used to add custom extensions to all Rangy Range objects.

For example, the following adds a convenient `insertNodeAtEnd()` method to all ranges:

```
rangy.rangePrototype.insertNodeAtEnd = function(node) {
    var range = this.cloneRange();
    range.collapse(false);
    range.insertNode(node);
    range.detach();
    this.setEndAfter(node);
};
```