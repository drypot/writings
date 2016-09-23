---
title: Filtering
---

    $(...).get(); // return array of DOM elements

    $(...).get(0); // return DOM element
    $(...)[0];

    $(...).eq(0); // return jQuery object
    $('...:eq(0)'); // slower

    $(...).filter(':even'); // same as $('tr:even')
    $(...).filter(function() {
      return true; // if returns false, removed from the matched
    });

    $(...).not(...);

    $(...).first(); // the first in the set.
    $(...).last();

    $(...).slice(2, 4); // 0-based

    $(...).has('ul'); // reduce to those that have a descendant

    $(event.target).is('button'); // true if at least one matches

    $(...).map(function() {
      return this.id;
    }).get().join();


{:class="go-to-index"}
[jQuery ABC](index)
