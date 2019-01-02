
# How the Rangy Range and Selection APIs differ from native browser APIs

Rangy implements the [DOM Level 2 Range specification](http://www.w3.org/TR/DOM-Level-2-Traversal-Range/ranges.html), the draft WHATWG Selection specification (formerly part of HTML5) and existing browser behaviour as closely as possible. However, while you can usually use exactly the same code with Rangy ranges and selections as you could with with the native APIs of standards-based browsers, there are a few key differences that prevent this always being the case. This page lists these differences.

## Ranges

### Range objects do not update when the DOM is changed, except when it is a range method doing the changing

This is the major stumbling block. The DOM4 Range specification does specify what should happen to a Range under DOM mutation and browsers do implement it. However, without [DOM mutation events](http://www.w3.org/TR/DOM-Level-2-Events/events.html#Events-eventgroupings-mutationevents) or [DOM mutation observers](https://dom.spec.whatwg.org/#mutation-observers) (both of which are absent in IE <= 8), it's simply impossible to detect all DOM mutation and therefore impossible for a Rangy range to update itself in response to DOM mutation. Therefore since the main goal of Rangy is to provide the same functionality in all supported browsers, Rangy makes no attempt to follow this aspect of the specification.

### None of the properties of a Range object throw an exception when assigned to

Simple solution: do not attempt to assign a value to a Rangy range property (`startContainer`, `endContainer`, `startOffset`, `endOffset`, `commonAncestorContainer` and `collapsed`), since it's not legal for native ranges anyway.

### Properties of a detached Range object throw no exceptions when accessed

This is also simply solved: do not attempt to interact with a detached Range.


## Selections

Rangy's `Selection` object is missing `modify()`, `setBaseAndExtent()` and `extend()` methods. This is because for `modify()`, the use case is better supplied by the [[TextRange module|Text Range Module]], and for other two methods, it is not possible to implement these in Internet Explorer up to and including version 11.