---
title: Animation
---

    $(...).show();
    $(...).hide(); // ~= .css('display', 'none'), remember old display property.
    $(...).toggle();

    $(...).show('slow'); // change width and height, 0.6 seconds.
    $(...).show('medium'); // 0.4 seconds.
    $(...).show('fast'); // 0.2 seconds.
    $(...).show(2000); // milliseconds
    $(...).show(fn);
    $(...).show(2000, fn);

    $(...).hide(...);
    $(...).toggle(...);

    $(...).fadeIn(...); // starts with full dimensions
    $(...).fadeOut(...);
    $(...).fadeToggle(...);
    $(...).fadeTo(0.5);

    $(...).slideDown(...);
    $(...).slideUp(...);
    $(...).slideToggle(...);

    $(...).animate({properties}, speed, easing, fn);
    $(...).animate({properties}, {options});

    $(...).animate({ height: 300, 'border-width': 10, borderHeight: 10 }, 'slow', function() {
      alert('The animation is finished.');
    });
    $(...).animate({ height: 'toggle' }, 'slow', fn);

    $(...).stop();

    if ($(...).is(':hidden')) { ... }


{:class="go-to-index"}
[jQuery ABC](index)
