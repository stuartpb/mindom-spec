# Minified DOM Interface (AKA DOM Bytecode) Specification

This defines a spec, sort of similar in spirit to WebAssembly, for a shorter-to-parse DOM interface.

## Motivation

The DOM is extremely verbose. While this is good from a non-ambiguity perspective, especially in contentious/rapidly-redefining edge standard spaces, for well-established methods, these extremely verbose method names are tedious and a waste of space.

We shouldn't go throwing incredibly-short versions of DOM methods around willy-nilly, but there *is* value in having them, especially for minified JS. Therefore, I propose UAs ship a facility, easily invoked with a minimal inline script, that provides this short common interface.

## Interface

This spec defines a function, tentatively named `window.MinDOM`, that, when invoked, adds a series of duplicate methods, getters, and setters to DOM prototypes for commonly used methods and properties of the DOM.

The function may also perform some kind of transformation to `<script>` elements on the page so as to switch them to use sources representing a further-minified source expecting these monkey-patched names. This mechanism is not yet defined at this stage of the specification.

## Monkey-patches

The names chosen for monkey-patching are picked to be fairly easy to recognize and/or remember, for debugging by hand or direct use, and to leave reasonable space for future short method/accessor names.

```js
// NOTE: this doesn't specify accessors vs. functions but you get the picture
d = document;
EventTarget.al = EventTarget.addEventListener;
Document.cE = Document.createElement;
Document.cF = Document.createDocumentFragment;
Element.cl = Element.classList;
Node.cN = Node.cloneNode;
Element.cn = Element.className;
Node.cp = Node.compareDocumentPosition
Document.dE = Document.documentElement;
Element.ga = Element.getAttribute;
Element.gb = Element.getBoundingClientRect;
Document.gi = Document.getElementById;
Node.od = Node.ownerDocument;
Node.pE = Node.parentElement;
Node.pN = Node.parentNode;
Document.q = Document.querySelector;
Document.qa = Document.querySelectorAll;
```

## Alternately

Rather than monkey-patching the DOM, the methods of `d` could return derived classes that implement these minified names, as well as (by virtue of inheritance) the full names (for less common / established properties and methods).

## Examples

```js
// before
var j = document.getElementById('foo').getAttribute('bar');
// after
var j=d.gi('foo').ga('bar');

```

## Observed effect

This cuts the size of minified jQuery down by somewhere around a third (before compression, rough estimate because I haven't actually specced this enough to try it).

## Polyfilling

This monkey-patching may be implemented by minification tools, to some degree, as a way to preserve most of the benefits of the shortened names in the case of repeated document access
