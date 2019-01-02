
[[Documentation home|Home]]

# Class Applier Module

This module adds, removes and toggles classes on all text nodes and optionally empty elements within a Range or Selection.

You can see a live example of this module in action [here](http://lezuse.github.io/rangy/demos/classapplier.html).

## Example

```
<script type="text/javascript">
    var applier;

    window.onload = function() {
        rangy.init();
        applier = rangy.createClassApplier("someClass");
    });
</script>

<input type="button" value="Toggle someClass on selection"
       onclick="applier.toggleSelection();">
```

## API

### Note about documentation formatting

Square brackets around one or more function or method parameters indicate that the parameter(s) is/are optional.

----

#### `rangy.createClassApplier(String theClass[, Object options[, Array tagNames]])`

##### `options` parameter new in v1.2
##### `tagNames` deprecated in favour of `tagNames` options from 1.2

Creates and returns a `ClassApplier` object. Elements created by this object are given the class `theClass`. Each property of the optional `options` parameter object is optional and uses the appropriate default value if not specified. The available properties are:

  * `elementTagName`: The tag name of the element to use for surrounding text. This may be any inline element that may contain text. The default is "span".
  * `elementProperties`: An object containing properties that are copied to each element created to surround text.
  * `elementAttributes`: An object containing attributes that are copied to each element created to surround text.
  * `ignoreWhiteSpace`: Boolean value indicating whether to ignore (i.e. not to wrap) insignificant white space text nodes (such as a line break and/or indentation between `<p>` tags in the HTML source of the page). The default is `true`.
  * `applyToEditableOnly`: Boolean value indicating whether to only apply the class to editable content (`contentEditable` true or `designMode` "on"). The default is `false`.
  * `tagNames`: a list of tag names corresponding to elements to which the class can be applied. This may either be an array (such as `["span", "b", "strong", "a"]`) or a comma-separated string (such as `"span, b, strong, a"`). If omitted, the default is `"span"`. You may alternatively specify `"*"` to indicate that the class may be applied to any element. However, block level elements (such as `<p>` and `<div>`) are ignored: when an element is partially selected, Rangy splits and copies the element, and if the element is block level, the layout will change.
  * `normalize`: Boolean value indicating whether the `ClassApplier` will merge adjacent text nodes and adjacent spans marked with its own unique class whenever it acts on a range or selection. The default is `true`.
  * `onElementCreate`: function that is called whenever a new element is created. Two parameters are passed to it: a reference to the element being created and a reference to the current class applier. Note that any changes to attributes or properties made by this function may prevent it from being merged with other elements.
  * `useExistingElements`: Boolean value indicating whether the `ClassApplier` will reuse existing elements when applying a class. The default is `true`.

### Notes on `elementProperties` and `elementAttributes`

- When using both `elementProperties` and `elementAttributes`, avoid setting the same property/attribute in both. Changing an attribute usually sets a property. Changing a property sometimes sets an attribute. Rangy does not check for this, so doing so will almost certainly lead to some kind of undesirable behaviour.
- In Internet Explorer <= 7 and emulation modes in later versions, `getAttribute()` and `setAttribute()` do not behave properly: they get/set a property rather than an attribute. This means that, for example, trying to add an event handler attribute such as `onclick` or an attribute represented by an object property such as `style` in `elementAttributes` will work inconsistently between IE <= 7 and other browsers. Rangy makes no attempt to correct for this.
- There is a special case in `elementProperties` for `style`: all properties of the `style` property in `elementProperties` are copied to the element's `style` property.
- `className` is also a special case in `elementProperties`: you can use it to add extra classes to elements created by Rangy. Don't include the main class for the applier.
- Don't try to set `class` or `className` in `elementAttributes`. They will be ignored.

## ClassApplier API

#### `applyToSelection([Window win])`

Surrounds all text nodes within the selection for the specified window with `<span>` elements with this ClassApplier's class. If no window is provided, the current window's selection is used.

----

#### `undoToSelection([Window win])`

Removes this ClassApplier's class from all text within the selection for the specified window. If no window is provided, the current window's selection is used.

----

#### `isAppliedToSelection([Window win])`

Returns a Boolean indicating whether all of the text contained within the selection for the specified window is contained within `<span>` elements with this ClassApplier's class. If no window is provided, the current window's selection is used.

----

#### `toggleSelection([Window win])`

Applies this ClassApplier's class to all of the text contained within the selection for the specified window unless all the text already has the class, in which case it is removed. If no window is provided, the current window's selection is used.

----

#### `applyToRange(Range range)`

Surrounds all text nodes within the specified Rangy range with `<span>` elements with this ClassApplier's class.

----

#### `undoToRange(Range range)`

Removes this ClassApplier's class from all text within the specified Rangy range.

----

#### `isAppliedToRange(Range range)`

Returns a Boolean indicating whether all of the text contained within the specified Rangy range is contained within `<span>` elements with this ClassApplier's class.

----

#### `toggleRange(Range range)`

Applies this ClassApplier's class to all of the text contained within the specified Rangy range unless all the text already has the class, in which case it is removed.

----

#### `detach([Document doc])`

##### Deprecated from v1.1

Removes this ClassApplier's unique class from all elements within the specified document. If no document is provided, the current document is used.

*Since no unique classes are used from version 1.1, this method no longer has any effect.*