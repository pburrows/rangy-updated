# DOM-related utility functions

Rangy uses some DOM helper functions internally, and provides these functions to other code via `rangy.dom`.

----

### Note about documentation formatting

Square brackets around one or more function or method parameters indicate that the parameter(s) is/are optional.

----

#### `rangy.dom.DomPosition(Node node, Number offset)`

Constructor function that creates an object representing a position within a document in terms of a DOM node and an offset within that node. For character nodes such as text, comment and CDATA nodes, this is a character offset within their text content. For all other nodes, the offset is the index of a child node within that node.

`DomPosition` objects have the following methods:

* `inspect()`: returns a string representation of the position, such as `[DomPosition(<P>:1)]`.
* `equals(DomPosition pos)`: returns a Boolean indicating whether this position is identical to the specified position.

----

#### `rangy.dom.comparePoints(Node nodeA, Number offsetA, Node nodeB, Number offsetB)`

Compares two positions (A and B) within a document and returns -1 if A is earlier in the document than B, 0 if A and B are identical or 1 if A is later in the document than B.

----

#### `rangy.dom.getBody(Document doc)`

Returns a reference to the body element of the specified document.

----

#### `rangy.dom.getClosestAncestorIn(Node node, Node ancestor[, Boolean selfIsAncestor])`

Returns a reference to the outermost node contained within `ancestor` that is an ancestor of `node`. Whether `node` is considered an ancestor of itself and is therefore a valid return value is determined by `selfIsAncestor`.

----

#### `rangy.dom.getCommonAncestor(Node node1, Node node2)`

Returns a reference to the innermost node that is an ancestor of both `node1` and `node2`.

----

#### `rangy.dom.getDocument(Node node)`

Returns a reference to the document that contains `node`.

----

#### `rangy.dom.getNodeIndex(Node node)`

Returns the index of the specified node within its parent.

----

#### `rangy.dom.getNodeLength(Node node)`

Returns the length of the specified node. For text and comment nodes, this is the length of the text the node represents. For any other node type, this is the number of child nodes of the node.

----

#### `rangy.dom.getWindow(Node node)`

Returns a reference to the Window object that contains `node`'s document.

----

#### `rangy.dom.insertAfter(Node node, Node precedingNode)`

Inserts the `node` immediately after `precedingNode` in the document and returns a reference to the inserted node.

----

#### `rangy.dom.inspectNode(Node node)`

Returns a string representation of the node for debugging purposes.

----

#### `rangy.dom.isAncestorOf(Node ancestor, Node descendant)`

Returns a Boolean indicating whether `ancestor` is an ancestor of `descendant`.

----

#### `rangy.dom.isOrIsAncestorOf(Node ancestor, Node descendant)`

Returns a Boolean indicating whether `ancestor` equals or is an ancestor of `descendant`.

----

#### `rangy.dom.isCharacterDataNode(Node node)`

Returns `true` if `node` is either a text node, comment node or CDATA node and `false` otherwise.

----

#### `rangy.dom.splitDataNode(Node node, Number index)`

Splits a character node into two at the character index specified by `index` and returns a reference to the newly created node, which is the node representing the content after the splitting point.

----

#### `rangy.dom.getIframeWindow(Element iframeEl)`

##### From v1.1

Returns the Window object for the specified `<iframe>` element.

----

#### `rangy.dom.getIframeDocument(Element iframeEl)`

##### From v1.1

Returns the Document object for the specified `<iframe>` element.

----

#### `rangy.dom.getRootContainer(Node node)`

##### From v1.2

Returns the root node for `node`.

----

#### `rangy.dom.getComputedStyleProperty(Element el, String propName)`

##### From v1.3

Returns the computed style property `propName` for element `el`.

----

#### `rangy.dom.getContentDocument(Mixed obj)`

##### From v1.3

Returns a Document object that varies depending of the parameter supplied:

  * If `obj` is an iframe element, return the content document for the iframe
  * If `obj` is a Window, return the document for that window
  * If `obj` is any other DOM node, return the node's owner document

----

#### `rangy.dom.removeNode(Node node)`

##### From v1.3

Removes `node` from its parent.

