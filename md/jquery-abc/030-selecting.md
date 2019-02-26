---
title: Selecting
---

    $('div'); // by tag
    $('div#id1'); // by id
    $('div.class1'); // by class

    $('div[height]'); // all tags that have a height
    $('div[height=200]');
    $('div[height!=200]');

    $('a[href^="mailto:"]'); // links with an href attribute that begins with mailto:
    $('a[href$=".pdf"]') // links with an href attribute that ends with .pdf
    $('a[href*="henry"]') // links with an href value that contains henry anywhere
    $('a[href^="http"][href*="henry"]') // combined

    $('div p'); // descendant p of div
    $('div > p'); // child p of div
    $('div, p, span'); // combine three result

    $('div:visible'); // by visibility
    $('div:hidden');

    $('div:first'); // by order
    $('div:last');
    $('div:even');
    $('div:odd'); // relative to selection
    $('div:eq(0)'); // zero-based
    $('div:gt(1)');
    $('div:lt(9)');
    $('div:nth-child(1)'); // css style, one-based
    $('div:nth-child(odd)'); // relative to parent

    $('div:first-child'); // if first child of their parent
    $('div:last-child');

    $('div:not(.horizontal)');

    $(':input'); // all input, select, textarea, button
    $(':button');
    $(':text'); // text typed input, but not textarea
    $(':radio');
    $(':checkbox');

    $(':enabled');
    $(':disabled');
    $(':checked');
    $(':selected');

    $('input[type="radio"]:checked');

    $('#news tr:has(td):not(:contains("topic"))');

    $('#news').find('tr:has(td)').not(function() {
      return $(this).children(':nth-child(4)').text() == 'topic';
    });


{:class="go-to-index"}
[jQuery ABC](index)
