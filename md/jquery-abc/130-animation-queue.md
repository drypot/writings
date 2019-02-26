---
title: Animation Queue
---

    $(...).animate({
        property1: 'value1',
        property2: 'value2'
      }, {
        duration: 'value',
        easing: 'value',
        specialEasing: {
          property1: 'easing1',
          property2: 'easing2'
        },
        complete: function() {
        alert('The animation is finished.');
      },
      queue: true, // 에니메이션을 체이닝, false 는 동시 실행
      step: callback
    });

    $(...)
    .css({position: 'relative'})
    .fadeTo('fast', 0.5)
    .animate({
      left: paraWidth - switcherWidth
    }, {
      duration: 'slow',
      queue: false
    })
    .fadeTo('slow', 1.0)
    .slideUp('slow')
    .queue(function(next) { // css() 같은 비 에니메이션 기능을 체이닝
      $switcher.css({backgroundColor: '#f00'});
     next();
    })
    .slideDown('slow');


{:class="go-to-index"}
[jQuery ABC](index)
