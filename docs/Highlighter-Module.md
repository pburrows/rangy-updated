
[[Documentation home|Home]]

# Highlighter module

Provides an API for adding, removing, serializing and deserializing highlights for static HTML based on the user selection.

Keeping track of highlight positions is done using a character offset-based technique.

Highlighting is done using the [[class applier module|Class Applier Module]] module.

You can see a live example of this module in action [here](http://lezuse.github.io/rangy/demos/highlighter.html).

## Dependencies

  * [[TextRange|Text Range Module]] (only required if using "TextRange" type Highlighter)
  * [[Class applier|Class Applier Module]]

## API

### Note about documentation formatting

Square brackets around one or more function or method parameters indicate that the parameter(s) is/are optional.

----

This module adds the following methods to the `rangy` object:

#### `rangy.createHighlighter([Document doc], [string type])`

Creates and returns a `Highlighter` object for the DOM `Document` object. If none is supplied, the current document is used.

The `type` parameter determines which algorithm is used for converting ranges to and from character offsets. It can have either of the following values:

  * *textContent*: A simple algorithm for generating character offsets from ranges based on all the text node content in the document. This approach has the advantage of being fast and reliable but the disadvantages of being susceptible to any changes to text nodes within the DOM, including collapsed white space characters and those in hidden elements, and of being much less likely to generate offsets that work in a browser other than the one in which they were created.

  * *TextRange*: If this value is used, the [[TextRange module|Text Range Module]] must be loaded. This is a more complicated approach. It has the advantage of being designed to take into account only characters that are visible on the page, making it more resilient to insignificant DOM changes and more likely to generate offsets that work in browsers other than the one in which they were created. However, it is slow. *Very* slow on a large DOM.

The default is "textContent".

----

## Highlighter API

#### `addClassApplier(ClassApplier applier)`

Adds a [[class applier|Class Applier Module]] that this highlighter can use to display highlights.

----

#### `highlightSelection(string className, [Object options])`

Highlights the current selection (or `options.selection`, if supplied), applying the class `className`. Any existing highlights that overlap the selection are merged into the new highlight, even if they are not using the same class.

A class applier for `className` must have previously been added to the highlighter using the `addClassApplier()` method.

Each property of the (optional) `options` parameter object is optional and uses the appropriate default value if not specified. The available properties are:

  * `selection`: The Rangy selection object to use for the new highlight. Defaults to the current selection in the document in which Rangy is running.
  * `exclusive`: Boolean. When set to `true`, this property prevents overlapping highlights. Defaults to `true`.
  * `containerElementId`: String. When specified, the highlight created is scoped to the element with this `id`, meaning that changes to other parts of the DOM cannot affect this highlight.
 
----

#### `unhighlightSelection([Selection selection])`

Removes all highlights that overlap the current selection (or `selection`, if supplied).

----

#### `removeAllHighlights()`

Removes all highlights.

----

#### `serialize([Selection selection])`

Serializes all highlights into a single string. This string may be passed into the `deserialize()` method on a different page visit.

----

#### `deserialize(string serialized)`

Recreates highlights that were previously serialized using the `serialize()` method.

----

#### `getHighlightForElement(Element el)`

Retrieves the `Highlight` object that contains the contents of element `el`.