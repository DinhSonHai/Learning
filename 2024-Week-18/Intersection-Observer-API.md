The Intersection Observer API provides a way to asynchronously observe changes in the intersection of a target element with an ancestor element or with a top-level document's viewport.

Use cases:
- Lazy-loading images or other contents as a page is scrolled.
- Implementing "infinity scroll" websites.
- Reporting of visibility of advertisements in order to calculate ad revenues.
- Deciding whether or not to perform task or animation processes based on whether or not the user will see the result.

The Intersection Observer API allows you to configure a callback that is called when either of these circumstances occur:
- A target element intersects either the device's viewport or a specified element. That specified element is called the root element or root for the purposes of the Intersection Observer API.
- The first time the observer is initially asked to watch a target element.

To watch for intersection relative to the device's viewport, specify null for the root option.
The degree of intersection between the target element and its root is the intersection ratio. This is a representation of the percentage of the target element which is visible as a value between 0.0 and 1.0.

[Intersection Observer options](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API#intersection_observer_concepts_and_usage:~:text=callback%20is%20invoked.-,Intersection%20observer%20options,-The%20options%20object)
- root: Must be the ancestor of the target. Defaults to the browser viewport if not specified or if null.
- rootMargin: Margin around the root. Can have values similar to the CSS margin property. Default to 0.
- threshold: Indicate when callback should be executed
    - Single number
    - Array of numbers: run every time visibility passed specify number. E.g. [0, 0.25, 0.5, 0.75, 1].

The callback receives a list of [IntersectionObserverEntry](https://developer.mozilla.org/en-US/docs/Web/API/IntersectionObserverEntry).

**Be aware that your callback is executed on the main thread. It should operate as quickly as possible; if anything time-consuming needs to be done, use Window.requestIdleCallback().**

Example:

    const options = {
        root: document.querySelector("#scrollArea"),
        rootMargin: "0px",
        threshold: 1.0,
    };
    const callback = (entries, observer) => {};
    const observer = new IntersectionObserver(callback, options);


Source: https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API
