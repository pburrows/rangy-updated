# Change log

*22 March 2015*

Rangy 1.3.0-beta.2 released. Second beta release. Changes since beta 1:

  * Add handling for non-HTML namespace elements (such as `<svg>`) to class applier (issue #272)
  * Prevent class applier applying a class to text within `<style>`, `<script>` and some other elements (issue #281)
  * Change `rangy.config.alertOnFail` default value to `false` (issue #280)
  * Fix for XHTML documents (PR #290)
  * Prevent a sneaky scenario where "Discontiguous selection is not supported" warnings are generated in Chrome (issue #276)
  * Change feature test elements to be inserted at the start of the body rather than the end in an attempt to fix issue #292
  * Add `type` option to highlighter `serialize()` method (PR #291)
  * Add `saveRanges()` and `restoreRanges()` methods to selection for issue #279

----
*12 February 2015*

Rangy 1.3.0-beta.1 released. First beta release for version 1.3. Changes since the last alpha release:

  * Class applier: now supports applying/unapplying classes to empty elements such as `<img>` (issue #83)
  * Class applier: improved support for `elementAttributes` option (issue #206, issue #213)
  * AMD modules now return a copy of the main Rangy object (pull request #270)
  * Minor bugfixes

----
*22 January 2015*

Rangy 1.3.0-alpha.20140921 released. Fixes various minor issues, mostly in the Class Applier module
Change Bower package name from rangy-official to rangy (thanks to Dom Barker for releasing the "rangy" package name)

----
*21 September 2014*

Rangy 1.3.0-alpha.20140921 released. Fixes the following issues:

  * Pull request #246: `findText()` bug when using regex (thanks to Ben Demboski)
  * Issue #248: IE 8 initializing too early
  * Issue #249: Error when using `selection` option in `highlightSelection()`

----
*27 August 2014*

Rangy 1.3.0-alpha.20140827 released. Internal tidying. No functional changes.

----
*25 August 2014*

Rangy 1.3.0-alpha.20140825 released. Includes a new feature:

  * `ignoreCharacters` option added to `characterOptions` used in TextRange module (issue #198)

Bugs fixed:

  * Global object issue #242

----
*22 August 2014*

Rangy 1.3.0-alpha.20140822.2 released. Fixed issue #241.

----
*22 August 2014*

Rangy 1.3.0-alpha.20140822 released. Added support for Browserify and Bower plus minor enhancements and bugfixes:

  * Fixed AMD issue #230
  * Rangy now auto-initializes if the document has already loaded and `rangy.config.autoInitialize` is true (thanks to Philip Peitsch)
  * Added `exclusive` option to highlighter module when highlighting and improved merging of highlights from pull request #228 (thanks to Vittorio)

----
*4 August 2014*

Rangy 1.3alpha.20140804 released. Fixed bug that meant all modules were broken in the last release.

----
*1 August 2014*

Rangy 1.3alpha.20140801 released. Contains minor enhancements and bugfixes:
                                 
  * AMD support (I think)
  * Suppressed "Discontiguous selection is not supported" errors in Chrome
  * Renamed rangy-cssclassapplier.js to rangy-classapplier.js
  * Added `rangy.config.autoInitialize` config option. Default is true
  * Added `getNativeTextRange()` method to selection
  * Modules now initialize automatically if core has already initialized
  * Build script now works on non-Windows platforms
  
----
*16 July 2014*

Rangy 1.3alpha.20140716 released. Contains minor enhancements and bugfixes and some changes to bring Range implementation in line with DOM4:

  * detach() is now a no-op
  * `RangeException` is gone; it's all `DOMException`s now.

----
*6 July 2014*

Rangy 1.3alpha.20140706 released. This version fixes issue 184, issue 186, issue 199 and issue 202.

----
*28 November 2013*

Rangy 1.3alpha.799 released. This version fixes issue 174, issue 176, issue 177, issue 188, issue 189 and issue 190.

----
*25 January 2013*

Rangy 1.3alpha.772 released.

----
*29 January 2013*

Rangy 1.3alpha.755 released.

----
*25 January 2013*

Rangy 1.3alpha.751 released. This adds an optional simpler algorithm to the Highlighter module, a `select()` method to Range and fixes various bugs in the previous alpha release.

----
*21 January 2013*

Rangy 1.3alpha.738 released. Includes a major rewrite of the TextRange module with the aims of fixing issue 115 and improving performance by building in lots of DOM caching where possible, plus a new Highlighter module that uses the TextRange module's character-based features to keep track of highlight positions. Also some new API methods and bugfixes.

----
*19 July 2012*

Rangy 1.3alpha.681 released. This fixes various bugs in the previous alpha release.

----
*23 June 2012*

Rangy 1.3alpha.675 released. This fixes various bugs in the previous alpha release.

----
*20 June 2012*

Rangy 1.3alpha.673 released. This fixes various bugs in the previous alpha release.

----
*11 June 2012*

Rangy 1.3alpha.650 released. This fixes various bugs in the previous alpha release.

----
*3 June 2012*

Rangy 1.3alpha.639 released. This fixes various bugs in the previous alpha release.

----
*28 May 2012*

Rangy 1.3alpha.603 released. The major new feature in 1.3 is the addition of the TextRange module.

----

*26 February 2012*

Rangy 1.2.3 released. This is a bugfix release that fixes issue 84 and issue 89.

----

*13 November 2011*

Rangy 1.2.2 released. This is a bugfix release that fixes issue 81.

----

*8 October 2011*

Rangy 1.2.1 released. This is a bugfix release that fixes issue 67 and issue 73.

----

*22 August 2011*

Version 1.2 released. This release adds enhancements to the CSS class applier module and a few new core methods, plus several bugfixes. Main features:

  * *CSS class applier module:* New option to use elements other than `<span>` for surrounding text
  * *CSS class applier module:* New option (enabled by default) to ignore non-significant white space nodes (for example, a line break and indentation between two `<p>` elements is ignored while a space between two `<span>` elements is not)
  * *CSS class applier module:* New option to apply CSS classes to editable content only (disabled by default)

  * *Core:* Added `toHtml()` method to both Range and Selection
  * *Core:* New Range methods `union()`, `isValid()`, `containsNodeText()` and `containsRange()`
  * *Core:* New option to allow IE 9 to use legacy `TextRange` and `document.selection` objects rather than standard W3C/WHATWG Range and Selection
  * *Core:* Range and Selection prototype objects exposed as `rangy.rangePrototype` and `rangy.selectionPrototype`, allowing extension
  * *Core:* Added `rangy.version` property

Issues fixed in this release since 1.1:

  * Issue 51, issue 53, issue 54, issue 57, issue 58, issue 60, issue 61, issue 62 and issue 64.

----

*29 May 2011*

Version 1.1.2 released. This is a bugfix release that fixes issue 46, issue 47, issue 48 and issue 50.

----

*15 May 2011*

Version 1.1.1 released. This is a bugfix release that fixes issue 41, issue 42 and issue 43.

----

*22 April 2011*

Version 1.1 released. The main focus of the 1.1 release is an overhaul to the CSS class applier module. Main features:

  * Unique class no longer generated for `CSSClassApplier` objects
  * Tag names parameter added to `rangy.createCssClassApplier()` method
  * CSS class applier module `isAppliedToRange()` and `isAppliedToSelection()` no longer have side effects
  * CSS class applier module now able to remove a class from a selection within an existing element with that class
  * Full support for IE 9 and Firefox 4
  * New `intersection()` and `equals()` methods for Range
  * Fixes for Range's `intersectsNode()` method
  * `rangy.getIframeSelection()` convenience method added, along with `rangy.dom.getIframeWindow()` and `rangy.dom.getIframeDocument()` utility methods
  * Fix for selection save/restore module (issue 32)
  * Fix to Serializer module that prevented it from enforcing checksum check (issue 40).

Issues fixed in this release since 1.0:

  * Issue 28, issue 30, issue 32, issue 34, issue 35, issue 37, issue 38 and issue 40.

----

*7 April 2011*

Version 1.1 beta 2 released. This release addresses minor bugs in 1.1 beta in IE 9 (issue 34, issue 35 and issue 38) and improves the selection save/restore module (issue 37). Documentation is also updated.

----

*12 March 2011*

Version 1.1 beta released. The only change since 1.1 alpha is a fix for issue 32.

----

*13 February 2011*

Version 1.1 alpha released. The main focus of this release is an overhaul to the CSS class applier module. Documentation updates will follow once more testing has been done. Main features:

  * Unique class no longer generated for `CSSClassApplier` objects
  * CSS class applier module `isAppliedToRange` and `isAppliedToSelection` no longer have side effects
  * CSS class applier module now able to remove a class from a selection within an existing element with that class
  * New `intersection()` and `equals()` methods for Range
  * `rangy.getIframeSelection()` convenience method added, along with `rangy.dom.getIframeWindow()` and `rangy.dom.getIframeDocument()` utility methods
  * Fixes for `Range.intersectsNode()`
  * rangy.dom.getIframeWindow
  * Fix for issue 28
  * Fix for issue 30

----

*2 January 2011*

Version 1.0.1 released. This is a bugfix release, primarily for a serious bug in the Save/restore module in 1.0. Changes:

  * Fix for issue 27
  * Fix for issue 26

----

*31 December 2010*

Version 1.0 released. Changes since 1.0 beta 2 are:

  * Fix for issue 24
  * Fix for issue 23
  * Fix for issue 21
  * Added `canSurroundContents()` method to Range objects
  * Added `checkForChanges` parameter to `refresh()` method of [[Selection|Rangy Selection]] objects
  * Various fixes for control selections in IE
  * Fixes for control selections and selections containing multiple ranges in [[Save/restore module|Save Restore Module]]
  * Moved `DOMException` from `rangy.dom` to `rangy`
  * Moved `RangeException` from `rangy.DomRange` to `rangy`

----

*21 November 2010*

Uploaded 1.0 beta 2 build. This build provides:
  * A fix for issue 19
  * Added `rangy.createMissingNativeApi()` method, which provides implementations of `document.createRange()` and `window.getSelection()` in browsers that don't have them (IE 6-8)
  * Added `canDeserializeRange()` and `canDeserializeSelection()` methods to [[Serializer module|Serializer Module]]

----

*10 November 2010*

Uploaded 1.0 beta 1 build. This provides a workaround for issue 17 and makes checksums optional in the [[Serializer module|Serializer Module]]. Module file names are now prefixed with `rangy-`. No new functionality will now be added before the 1.0 release.

----

*4 November 2010*

New build uploaded. This build fixes a bug in IE TextRange to Range conversion and fixes a bug introduced in 0.1.201 that added a test text node to the document.

----

*3 November 2010*

New build uploaded. This build fixes issue 16 and improves the [[Serializer module|Serializer Module]] to use checksums and allow serialization relative to a root node.

----

*31 October 2010*

New build uploaded. Everything is now packaged in zip and tar.gz format. New build includes [[Serializer module|Serializer Module]] and the core contains changes to support the Serializer module.

Also created a [Google Group for Rangy discussion](http://groups.google.com/group/rangy).

----

*29 October 2010*

New build (revision 184) of the core uploaded. This contains a fix for issue 14.

----

*26 October 2010*

New build (revision 181) of the core uploaded. This contains bugfixes for the `normalizeBoundaries()` method of range objects.

Also uploaded a new version of the [[selection save/restore module|Selection Save Restore Module]] with an optimization for insertion points and API moved to the `rangy` object rather than the previous separate `rangy.saveRestore` object.

----

*23 October 2010*

New build (revision 166) of the core uploaded. This contains a fix for issue 12.

----

*21 October 2010*

New build (revision 160) of the core uploaded. This just contains a fix for issue 11.

----

*20 October 2010*

New build (revision 153) of the core uploaded. This contains a fix for issue 10, plus:

 * Fixed issue 10 by creating workaround for IE TextRange parentElement() bug
 * Added collapseBefore() and collapseAfter() methods to range objects
 * Added setSingleRange() to selection objects
 * Improved IE favouring of text nodes when converting collapsed TextRange to Range.

----

*19 October 2010*

New build (revision 148) of the core uploaded. This contains a fix for issue 9.

----

*18 October 2010*

New build (revision 146) of the core released. Features:

 * Fixes issue 7;
 * improved initialization;
 * `inspect()` methods added to Range and Selection objects;
 * Much improved algorithm for converting `TextRange` to `Range` within a preformatted text node with line breaks in IE;
 * Adds `refresh()` method to `WrappedRange` objects;
 * Fixed detection of whether selections can support multiple ranges;
 * Improved `setRanges()` method of selection objects to allow "Control" type selections in IE.

I've also written a large chunk of documentation in the Wiki area that's still unfinished but better than none, which is what there was previously.

----

*6 October 2010*

Released new build of the core. Most core changes are on the Selection object. Documentation progressing slowly but will get done soon, I promise.

I also uploaded CSS Class Applier module. I plan to make a much more generic act-on-range-nodes module but that requires quite a lot of thought, and the CSS Class Applier works now. You can use this module to create a `CssClassApplier` object that will apply, remove and toggle a particular CSS class on a range or selection by adding and removing `<span>` elements. It will also optionally normalize nodes it affects as it adds or removes a CSS class (i.e. merging adjacent text nodes or identical `<span>`s).

Example usage;

```
<script type="text/javascript">
    var cssApplier;

    rangy.addInitListener(function(rangy) {
        cssApplier = rangy.createCssClassApplier("someClass", true); // true turns on normalization
    });
</script>

<input type="button" value="Toggle someClass on selection" onclick="cssApplier.toggleSelection();">
```

To keep track of `<span>`s that it has created, the `CssClassApplier` adds an extra unique class to each `<span>`. You can remove these when you have finished with the `CssClassApplier` by calling its `detach()` method:

```
cssApplier.detach();
```

----

*21 August 2010*

Released new builds of the core and save/restore module, fixing several bugs.

----

*12 August 2010*

Uploaded initial selection save and restore module. Include it after the main rangy-core.js file. Example usage:

Save selection:
```
var savedSelection = rangy.saveRestore.saveSelection();
```


Restore selection:
```
rangy.saveRestore.restoreSelection(savedSelection);
```

----

*2 August 2010*

Uploaded pre-alpha version of Rangy core.