
[[Documentation home|Home]]

# Properties and methods of the main rangy object

Some Rangy modules add extra methods to the main `rangy` object. These methods are documented in the documentation for each module. The following are the properties and methods of the core `rangy` object.

### Note about documentation formatting

Square brackets around one or more function or method parameters indicate that the parameter(s) is/are optional.

## Properties

----

#### `rangy.config`

An object containing configuration properties:

  * `alertOnFail` (*from v1.3*): Boolean indicating whether to show an alert when Rangy fails to initialize. Defaults to `false`.
  * `alertOnWarn`: Boolean indicating whether Rangy's warning messages, which are usually sent to the browser's console (if present), are also alerted to the user. Defaults to `false`.
  * `checkSelectionRanges`: Boolean indicating whether any range added to a Rangy Selection is checked as it is added. See the [addRange() documentation](RangySelection#addRange(Range_range)) for details. Defaults to `true`.
  * `preferTextRange` (*from v1.2*): Boolean indicating whether legacy `TextRange` and `document.selection` objects should be used internally rather than standard Range and Selection objects in browsers that have both (IE 9 and above). Set this option to `true` if you need Rangy in IE 9 and above to behave the same as in previous versions of IE. Defaults to `false`.
  * `autoInitialize` (*from v1.3*): Boolean indicating whether Rangy should automatically initialize when the document loads. Defaults to `true`. This value can also be set using a global variable called `rangyAutoInitialize`.

----

#### `rangy.dom`

An object containing [[DOM-related utility functions|Dom Utils]].

----

#### `rangy.features`

An object containing information about native browser support for `Range`, `TextRange` and `Selection` objects. It has the following properties:

  * `collapsedNonEditableSelectionsSupported`: Boolean indicating whether the browser allows collapsed selections that are not contained within an editable element or document.
  * `implementsControlRange`: : Boolean indicating whether the browser implements [ControlRange](http://msdn.microsoft.com/en-us/library/ms537447%28VS.85%29.aspx).
  * `implementsDomRange`: Boolean indicating whether the browser implements [DOM Level 2 Range](http://www.w3.org/TR/DOM-Level-2-Traversal-Range/ranges.html).
  * `implementsTextRange`: Boolean indicating whether the browser implements [TextRange](http://msdn.microsoft.com/en-us/library/ms535872%28VS.85%29.aspx).
  * `selectionHasAnchorAndFocus`: Boolean indicating whether the browser's selection object has `anchorNode`, `anchorOffset`, `focusNode` and `focusOffset` properties.
  * `selectionHasExtend`: Boolean indicating whether the browser's selection object has an `extend()` method.
  * `selectionHasRangeCount`: Boolean indicating whether the browser's selection object has the `rangeCount` property.
  * `selectionSupportsMultipleRanges`: Boolean indicating whether the browser's selection object supports multiple Ranges.
  * `crashyTextNodes` (*from v1.3*): Boolean indicating whether the browser has a particular bug that causes it to throw an error when the `parentNode` property of a dead text node is accessed
  * `implementsDocSelection` (*from v1.3*): Boolean indicating whether the browser implements `document.selection` (primarily IE 6 - 10)
  * `implementsWinGetSelection` (*from v1.3*): Boolean indicating whether the browser implements `window.getSelection()`

----

#### `rangy.initialized`

Boolean indicating whether Rangy has been initialized yet (which it does automatically once the page has loaded).

----

#### `rangy.modules`

Object containing Rangy modules.

----

#### `rangy.supported`

Boolean indicating whether the browser supports the required functionality for Rangy to work.

----

#### `rangy.util`

Object containing general utility functions used by Rangy:

  * `isHostObject()`, `isHostMethod()`, `isHostProperty()`: Feature detection functions originally taken from [an article by Peter Michaux](http://michaux.ca/articles/feature-detection-state-of-the-art-browser-scripting).
  * `areHostObjects()`, `areHostMethods()`, `areHostProperties()`: Convenience feature detection functions identical to `isHostObject()`, `isHostMethod()` and `isHostProperty()` respectively apart from taking an array of properties to test rather than a single property.
  * `toArray()` (*from v1.3*) converts an array-like object into an actual Array.

----

#### `rangy.DomRange(Document doc)`

Constructor function for Rangy's pure JavaScript Range implementation. Used internally and not intended for use by other code. This function also has a number of useful methods:

  * `inspect(Range range)`: Returns a string representation of the specified Range, which may be a Rangy Range or a native Range.
  * `rangesEqual(Range range1, Range range2)`: Compares two Ranges (which may be any combination of Rangy Ranges or native Ranges) and returns a Boolean indicating whether they represent the same part of the same document.


----

#### `rangy.Selection(Selection selection)`

Constructor function for Rangy's Selection implementation. Used internally and not intended for use by other code. This function also has a method:

  * `inspect(Selection sel)`: Returns a string representation of the specified Selection object, which may be a Rangy Selection or a native Selection.

----

#### `rangy.WrappedRange(Object range)`

Constructor function for Rangy's main Range implementation (as created by `rangy.createRange()`). Used internally and not intended for use by other code.

----

## Methods

----

#### `rangy.addInitListener(Function listener)`

Adds a function to a list of listeners that are called when Rangy is initialized. If Rangy is already initialized, the listener function is called immediately.

The listener function is passed a single parameter, which is a reference to the main `rangy` object.

----

#### `rangy.shim()`

Provides implementations of `document.createRange()` and `window.getSelection()` in browsers that don't have them (primarily Internet Explorer 6-8). It creates a `window.getSelection()` method that returns a Rangy selection and creates a `document.createRange()` method that creates a Rangy range. In browsers that have these methods already, nothing happens. Therefore in theory you could call `rangy.shim()` and existing Range/Selection manipulation code will magically start working in IE 6-8.

However, this is not the preferred way of using Rangy for the following reasons:

  * Involves modifying host objects (`window` and `document`), which is [generally a bad idea](http://perfectionkills.com/whats-wrong-with-extending-the-dom/);
  * IE gets Rangy Range and Selection objects, which [[behave slightly differently from native objects|How Rangy Differs From Native Apis]], while other browsers keep their variously bug-ridden native Range and Selection objects, meaning that things will not necessarily work uniformly. However, in many cases it may well be good enough.

----

#### `rangy.createMissingNativeApi()`

Alias for `rangy.shim()`.

----

#### `rangy.createNativeRange([Document doc])`

Creates and returns a native browser Range or TextRange for the specified document. If no document is provided, the current document is used. From version 1.3, a Window object or iframe element may be used instead.

----

#### `rangy.createRange([Document doc])`

Creates and returns a [[Rangy Range|Rangy Range]] object for the specified document. If no document is provided, the current document is used. From version 1.3, a Window object or iframe element may be used instead.

This is the main method for creating Ranges.

----

#### `rangy.createRangyRange([Document doc])`

Creates and returns a pure JavaScript Rangy Range object for the specified document. If no document is provided, the current document is used. From version 1.3, a Window object or iframe element may be used instead.

Use this in preference to `rangy.createRange()` if you want to be certain of identical behaviour between browsers. The range returned by `rangy.createRange()` wraps a native browser Range or TextRange and uses methods of the native object wherever possible for speed; the range `rangy.createRangyRange()` returns uses no native browser Range or TextRange object, giving the reassurance of a single code path at the expense of some performance.

----

#### `rangy.getNativeSelection([Window win])`

Returns the native browser selection object for the specified window. If no window is provided, the current window is used. From version 1.3, a Document object or iframe element may be used instead.

----

#### `rangy.getSelection([Window win])`

Creates and returns a Rangy selection object for the specified window object. If no window is provided, the current window is used. From version 1.3, a Document object or iframe element may be used instead.

This method also has the effect of refreshing the Rangy selection for the specified window.

----

#### `rangy.getIframeSelection(Element iframeEl)`

##### From v1.1

Convenience method for creating a Selection object for the specified `<iframe>` element.

This method also has the effect of refreshing the Rangy selection for the specified `<iframe>` element.

This method is deprecated from version 1.3. Use `rangy.getSelection(iframeEl)` instead.

----

#### `rangy.init()`

Initializes Rangy if it has not already been initialized. Rangy initializes itself automatically once the document has loaded but this method ensures that Rangy is ready to use. It's useful in a `load` or `DOMContentLoaded` event handler, such as one provided by jQuery's `$(document).ready()`, or when Rangy's script is loaded after the document has loaded.

##### Example

```
window.onload = function() {
    rangy.init();
    var sel = rangy.getSelection();
    sel.selectAllChildren(document.body);
};
```

----

#### `rangy.isSelectionValid([Window win])`

##### From v1.1.2

Checks whether the range(s) extracted from the selection for the specified window are actually located within that window's document. This is to work around a quirk in Internet Explorer, which returns a collapsed range within the main document when attempting to retrieve the selection from an iframe which had a collapsed selection prior to losing the focus.

If no window is provided, the current window is used. From version 1.3, a Document object or iframe element may be used instead.