# jquery-pagerizer

- [INSTALL](#install)
- [SYNOPSIS](#synopsis)
- [DESCRIPTION](#description)
- [INSTANCE METHODS](#instance-methods)
  - [addRel](#addrel)
  - [findRelLinks](#findrellinks)
  - [removeRel](#removerel)
- [COMPATIBILITY](#compatibility)
- [SEE ALSO](#see-also)
- [AUTHOR](#author)
- [COPYRIGHT AND LICENSE](#copyright-and-license)

A jQuery plugin to add next/previous page annotations to web pages

## INSTALL

Save the minified file shipped in the the `dist` directory, or use a CDN e.g.:

* [RawGit](https://cdn.rawgit.com/chocolateboy/jquery-pagerizer/v1.0.0/dist/pagerizer.min.js)
* [GitCDN](https://gitcdn.xyz/repo/chocolateboy/jquery-pagerizer/v1.0.0/dist/pagerizer.min.js)

## SYNOPSIS

```javascript
// ==UserScript==
// @name          Pagerize Example
// @description   Mark up Example.com result pages with pager annotations
// @version       0.0.1
// @include       http://www.example.com/examples/*
// @require       https://code.jquery.com/jquery-3.3.1.min.js
// @require       https://cdn.rawgit.com/chocolateboy/jquery-pagerizer/v1.0.0/dist/pagerizer.min.js
// ==/UserScript==

$('a#older').addRel('prev')
$('a#newer').addRel('next')
```

## DESCRIPTION

jQuery Pagerizer is a [jQuery](https://jquery.com/) plugin which exposes helper methods that can be used to
easily add pager annotations to web pages with missing or incorrect `rel="prev"` and `rel="next"` attributes.
These annotations can then be consumed by an addon or userscript which allows pages to be navigated
by keyboard e.g. the <kbd>[[</kbd> and <kbd>]]</kbd> key bindings in [Tridactyl](https://github.com/cmcaine/tridactyl), [Vim Vixen](https://github.com/ueokande/vim-vixen) &c.

Existing `rel` values are read in a case-insensitive way, but new `rel` values preserve the supplied case e.g.:

```javascript
$('<a rel="NEXT"></a>').addRel('next') // <a rel="NEXT"></a>
$('<a rel="quux"></a>').addRel('Prev') // <a rel="quux Prev"></a>
```

Although designed to facilitate the manipulation of pager annotations, the plugin can be used to add or remove
any `rel` attributes with the expected "string set" semantics.

```javascript
$('<a></a>').addRel('foo')                      // <a rel="foo"></a>
$('<a></a>').removeRel('foo')                   // <a></a>
$('<a rel="foo"></a>').addRel('foo')            // <a rel="foo"></a>
$('<a rel="foo foo"></a>').addRel('bar')        // <a rel="foo bar"></a>
$('<a rel="foo foo"></a>').removeRel('foo')     // <a></a>
$('<a rel="foo foo bar"></a>').removeRel('foo') // <a rel="bar"></a>
```

## INSTANCE METHODS

### addRel

**Signature**: addRel(rels: ...String) → JQuery

```javascript
$('a#older').addRel('prev')
$('a#newer').addRel('next')
```

Add the specified `rel` values to the matching element or elements.

### findRelLinks

**Signature**: findRelLinks(rels: ...String) → JQuery

```javascript
const $existingLinks = $(document).findRelLinks()
const $onlyNextLinks = $(document).findRelLinks('next')
```

Return all A and LINK elements that have `rel` attributes which include any of the supplied values.
If no `rel` values are supplied, the call is equivalent to:

```javascript
findRelLinks('prev', 'previous', 'next')
```

### removeRel

**Signature**: (rels: ...String) → JQuery

```javascript
// remove all next/previous-page rel links
$(document).findRelLinks().removeRel()

// remove the next-topic link so we can replace it with a next-page link
$('a.next-topic').removeRel('next')
$('a.current-page').next().addRel('next')
```

Removes the specified `rel` values from the `rel` attribute and returns the jQuery instance.
If no `rel` values are supplied, the call is equivalent to:

```javascript
removeRel('next', 'prev', 'previous')
```

If all `rel` values are removed, the `rel` attribute is removed:

```javascript
$('<a rel="foo"></a>').removeRel('foo') // <a></a>
```

## COMPATIBILITY

* This plugin should work in any browser with ES5 support.
* It has been tested with jQuery 3.x, and may not work with earlier versions.
* It has been tested on Greasemonkey 3, but should work in all current userscript engines.

## SEE ALSO

* [Pagerizer Userscripts](https://github.com/chocolateboy/userscripts#pagerizers)
* [The Stigmergic Userscript Pattern](https://ecmanaut.blogspot.co.uk/2006/04/stigmergic-user-script-pattern.html)
* [A Survey of Rel Values on the Web](http://blog.unto.net/a-survey-of-rel-values-on-the-web.html)

## AUTHOR

[chocolateboy](mailto:chocolate@cpan.org)

## COPYRIGHT AND LICENSE

Copyright © 2013-2018 by chocolateboy

jquery-pagerizer is free software; you can redistribute it and/or modify it under the terms
of the [Artistic License 2.0](http://www.opensource.org/licenses/artistic-license-2.0.php).
