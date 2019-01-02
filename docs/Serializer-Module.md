
[[Documentation home|Home]]

# Serializer module

Serializes and deserializes ranges and selections. It can be used to store the user's selection on a particular page in a cookie, local storage or the server and restore it on the user's next visit to the same page. The module includes methods to save and restore a selection using a cookie.

The serialization technique is to serialize the start and end boundaries of each range within the selection. Each boundary is serialized as a sequence of node offsets, starting with the boundary container node and working up the DOM tree to the root node specified in the serialization call (or the `Document` element at the top if no root node is specified). This makes this technique somewhat fragile. It requires:

 * The DOM must be in the same state when the selection is serialized and deserialized. For a static page this unlikely to be a problem. For a dynamic page with animations or dynamically loaded content, this could be more difficult.
 * A serialized position may not deserialize correctly in a browser different to the one in which it was serialized. In particular, IE's DOM is significantly different to that of other browsers so a serialized selection from IE is highly unlikely to deserialize correctly in other browsers, and vice versa.

Because the technique is so fragile, a serialized range optionally includes a checksum calculated based on the DOM subtree within the root element for serialization. When deserializeRange() is called, it compares this checksum with a checksum of the root node specified for deserialization and throws an error if they do not match.

You can see a live example of this module in action [here](http://rangy.googlecode.com/svn/trunk/demos/serializer.html).

## Example

The following example persists the user's selection across requests to the same page:

```
function restoreSelection() {
    rangy.init();
    rangy.restoreSelectionFromCookie();
}

var selectionSaved = false;

function saveSelection() {
    if (!selectionSaved) {
        rangy.saveSelectionCookie();
        selectionSaved = true;
    }
}

window.onload = restoreSelection;
window.onbeforeunload = saveSelection;
window.onunload = saveSelection;
```

The example uses `rangy.restoreSelectionFromCookie()` and `rangy.saveSelectionCookie();` to save the selection in the current window to a cookie when the page unloads and restore the selection when the page loads.

## API

### Note about documentation formatting

Square brackets around one or more function or method parameters indicate that the parameter(s) is/are optional.

----

This module adds the following methods to the `rangy` object:

#### `rangy.serializeSelection([Selection sel[, Boolean omitChecksum, [Element rootNode]])`

Serializes the specified selection object (which may be a Rangy selection or native browser selection) relative to `rootNode` and returns a string. If no selection object is provided, the selection for the current window is used. If `rootNode` is not provided, serialization is relative to the document's root element.

For a large document, you may wish to set `omitChecksum` to `true` because calculating the checksum is quite expensive: if no root node is set, the checksum generation function traverses the whole document.

----

#### `rangy.canDeserializeSelection(String serializedSelection[, Element rootNode[, Window win]])`

Returns a Boolean indicating whether the specified serialized selection string can be deserialized successfully relative to `rootNode`. If no window object is provided, the current window is used. If `rootNode` is not provided, deserialization is relative to the document's root element.

----

#### `rangy.deserializeSelection(String serializedSelection[, Element rootNode[, Window win]])`

Deserializes a serialized selection relative to `rootNode` and updates the current selection. If no window object is provided, the current window is used. If `rootNode` is not provided, deserialization is relative to the document's root element.

N.B. If the state of the DOM is different from when the selection was originally serialized, this method may not work.

----

#### `rangy.serializeRange(Range range[, Boolean omitChecksum, [Element rootNode]])`

Serializes the specified range object (which may be a Rangy range or native browser `Range`) relative to `rootNode` to a string and returns it. If `rootNode` is not provided, serialization is relative to the document's root element.

For a large document, you may wish to set `omitChecksum` to `true` because calculating the checksum is quite expensive: if no root node is set, the checksum generation function traverses the whole document.

----

#### `rangy.canDeserializeRange(String serializedRange[, Element rootNode[, Document doc]])`

Returns a Boolean indicating whether the specified serialized Range string can be deserialized successfully relative to `rootNode`. If no document is provided, the current document is used. If `rootNode` is not provided, deserialization is relative to the document's root element.

----

#### `rangy.deserializeRange(String serializedRange[, Element rootNode[, Document doc]])`

Deserializes a serialized range relative to `rootNode` and returns the deserialized Range within the specified document. If no document is provided, the current document is used. If `rootNode` is not provided, deserialization is relative to the document's root element.

N.B. If the state of the DOM is different from when the Range was originally serialized, this method may not work.

----

#### `rangy.serializePosition(Node node, Number offset[, Element rootNode])`

Serializes the position represented by the specified offset within the specified node relative to `rootNode` to a string and returns it. If `rootNode` is not provided, serialization is relative to the document's root element.

----

#### `rangy.deserializePosition(String serializedPosition[, Element rootNode[, Document doc]])`

Deserializes a serialized position relative to `rootNode` and returns the deserialized `DomPosition` (containing `node` and `offset` properties) within the specified document. If no document is provided, the current document is used. If `rootNode` is not provided, deserialization is relative to the document's root element.

N.B. If the state of the DOM is different from when the `DomPosition` was originally serialized, this method may not work.

----

#### `rangy.saveSelectionCookie([Window win, [Object props]])`

Serializes the selection within the specified window to a string and stored it in a cookie. If no window object is provided, the current window is used.

The properties of the cookie that is stored may be customized by the optional `props` object. By default, the cookie only applies to the current page and expires at the end of the browser session. You can change this with the following properties of `props`:

 * `expires`: a `Date` object specifying when the cookie will expire;
 * `path`: a string specifying the path for which the cookie is valid;
 * `domain`: a string specifying the domain for which the cookie is valid;
 * `secure`: a Boolean specifying whether the cookie can is only valid for secure requests (using SSL, for example).

----

#### `rangy.restoreSelectionFromCookie([Window win])`

Deserializes a serialized selection saved in a cookie by `saveSelectionCookie` and updates the selection for the window specified. If `win` is not specified, the current window is used.

N.B. If the state of the DOM is different from when the selection was originally serialized, this method may not work.

----

## Serialization format

An example serialized selection is as follows:

```
14/1/3:62,14/1/3:66{f910c7cc}|0/1/3/3:4,0/1/7/3/3:10{f910c7cc}
```

The serialization used has the following structure:

### Selection

A serialized selection comprises one or more (for the case of multiple selected ranges in Firefox) serialized ranges separated by pipe (`|`) characters.

### Range

A serialized range has the structure


```
start,end{checksum}
```

... where `start` and `end` are serialized positions representing the start and end of the range and checksum is an optional checksum representing the state of the DOM at the time of serialization.

### Position

A serialized position has the structure

```
node_path:index
```

... where `node_path` is a list of child node indices separate by `/`. The list starts from the index of the deepest node containing the position followed by the index of each ancestor in turn, up to the root node in the document (not the document itself).

For example, for the following HTML document

```
<html><head></head><body>One <b>two</b> <i>thre|e</i></body></html>
```

... the position represent by the `|` character serializes to

```
0/3/1:4
```

... because the position is after 4 characters in the text node "three", which is at index 0 of `<i>`'s child nodes, which is itself at index 3 of `<body>`'s child nodes (preceded by "One ", `<b>` and " "), which is index 1 of `<html>`'s child nodes, and `<html>` is the root element of the document.

### Utility methods

This module adds a `crc32()` method to `rangy.util` which can be used as follows:

```
var checksum = rangy.util.crc32("Some string");
```