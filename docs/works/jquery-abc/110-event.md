---
title: Event
---

jQuery always registers event handlers for the bubbling phase, not capturing.

    $(...).on(events, [selector], [data], handler); // use 'on' instead of bind, delegate, live.

    $('... tbody').on('click', 'tr', { key: value }, function(event){
      this; // the DOM element to which the behavior was attached
      $(this);
      event.originalEvent;
      event.type; // 'click', 'resize', ...
      event.target; // event occurred element
      event.data.key;
      if (event.target == this) {
        // ensures that the clicked on was not one of its sub-elements.
      }
      return false; // event.stopPropagation() + event.preventDefault()
    });

    $(...).trigger('click');

    $(...).click(fnunction () { ... });
    $(...).dblclick(fn);

    $(...).mouseenter(fn); // don't bubble
    $(...).mouseleave(fn);

    $(...).hover(fn, fn); // mouseenter + mouseleave

    $(...).mouseover(fn); // bubble
    $(...).mouseout(fn);

    $(...).mousedown(fn);
    $(...).mouseup(fn);
    $(...).mousemove(fn);

    $(...).focus(fn);
    $(...).blur(fn);

    $(...).keydown(fn);
    $(...).keyup(fn);
    $(...).keypress(fn);

    $(...).toggle(fn, fn); // deprecated

    $(...).on('click.abc', ...); // event namespacing


{:class="go-to-index"}
[jQuery ABC](index)
