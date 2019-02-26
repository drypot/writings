---
title: Traversing
---

    $(...).index(); // the position of the first element in relation to its siblings

    $(...).each(function (index, e) {
    $(e).css(...);
    $(this).css(...); // same
    });

    $(...).siblings(...);
    $(...).next(...); // the very next sibling element
    $(...).nextAll(...); // all the following
    $(...).prev(...);
    $(...).prevAll(...);

    $(...).parent(...);
    $(...).parents(...);
    $(...).closest(...); // testing the element itself and traversing up through its ancestors

    $(...).children(...);

    $(...).find(...); // the descendants of each element in the current set

    $(...).add(...); // create new union set

    $(...).end();
    $(...).andSelf();


{:class="go-to-index"}
[jQuery ABC](index)
