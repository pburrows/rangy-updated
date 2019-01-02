
# New features in Rangy 1.2

## Summary

### CSS Class Applier module improvements

  * Added option to use elements other than `<span>` for surrounding text
  * Added option (enabled by default) to ignore non-significant white space nodes (for example, a line break and indentation between two `<p>` elements is ignored while a space between two `<span>` elements is not)
  * Added option to apply CSS classes to editable content only (disabled by default)
  * Range and Selection prototype objects exposed, allowing extension
  * Some major bugfixes (issue 54, issue 57, issue 60, issue 61 and issue 64). 

### Core additions

  * Added `toHtml()` method to both Range and Selection
  * Added `union()` method to Range
  * Added `isValid()` method to Range
  * Added `containsNodeText()` and `containsRange()` methods to Range
  * Added option to allow IE 9 to use legacy `TextRange` and `document.selection` objects rather than standard W3C/WHATWG Range and Selection
  * Added `rangy.version` property

### Issues fixed

Issue 51, issue 53, issue 54, issue 57, issue 58, issue 60, issue 61, issue 62 and issue 64.

## CSS Class Applier change details

The second parameter in `rangy.createCssClassApplier()` is now an options object. It's backwards compatible, so providing a Boolean value will set the `normalize` option as before.

### `rangy.createCssClassApplier(String cssClass[, Object options[, Array tagNames]])`

Creates and returns a `CssClassApplier` object. Elements created by this object are given the CSS class `cssClass`. The available properties of the `options` parameter object are as follows:

  * `elementTagName`: The tag name of the element to use for surrounding text. This may be any inline element that may contain text. The default is "span".
  * `elementProperties`: An object containing properties that are copied to each element created to surround text
  * `ignoreWhiteSpace`: Boolean value indicating whether to ignore insignificant white space text nodes (such as a line break and/or indentation between `<p>` tags in the HTML source of the page). The default is `true`.
  * `applyToEditableOnly`: Boolean value indicating whether to only apply the CSS class to editable content (`contentEditable` true or `designMode` "on"). The default is `false`.
  * `normalize`: As per the `normalize` function parameter in Rangy 1.1.

#### Example

The following will create a CSS class applier that will surround a range or selection within a link (or links) to the Rangy home page, with class "someClass":

```
var applier = rangy.createCssClassApplier("someClass", {
    elementTagName: "a",
    elementProperties: {
        href: "http://code.google.com/p/rangy",
        title: "Rangy home page"
    }
});
```

## Core change details

### `toHtml()`

Called on a Range or Selection object. Returns a string containing an HTML representation of the current Range or Selection.

### `union(Range range)`

Called on a Range object. Providing `range` overlaps or touches the current range, returns the smallest possible Range containing the whole of both ranges, or null otherwise.

### `isValid()`

Called on a Range object. Returns a Boolean that is true if the range start and end boundaries are valid, i.e. the boundary container nodes still exist in the document and the boundary offsets are valid for the boundary nodes.

### `containsNodeText(Node node)`

Called on a Range object. Returns a Boolean representing whether the range contains all of the text (within text nodes) contained within `node`. This is to provide an intuitive means of checking whether a range "contains" a node if you consider the range as a visible text selection on the page, without having to worry about niggly details about range boundaries.

### `containsRange(Range range)`

Called on a Range object. Convenience method that returns a Boolean representing whether the current range completely contains `range`.

### `rangy.config.preferTextRange`

Setting this option to `true` causes Rangy to use legacy `TextRange` and `document.selection` objects internally in IE 9 rather than standard Range and Selection objects. You can use this option if you need Rangy in IE 9 to behave the same as in previous versions of IE.

### Range and Selection prototype objects

Prototype objects for Range and Selection are now exposed as `rangy.rangePrototype` and `rangy.selectionPrototype`. This allows custom extension of all ranges and selections.