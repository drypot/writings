---
title: Create, Insert, Move, Delete Tag
---

    $('<a href="#top">back to top</a>'); // create new elements

    $('<tag></tag>', {
      id: '...',
      'class': '...',
      text: '...',
      data: {'index': ...},
      click: function() {
        return false;
      },
      css: {glowColor: '#00ff00'}
    }).insertBefore($table);

    $('<b>html</b>').insertBefore('#target'); // insert outside of the target
    $('<b>html</b>').insertAfter('#target');

    $('<b>html</b>').prependTo('#target'); // insert inside of target
    $('<b>html</b>').appendTo('#target');

    $('#container').before('<b>html</b>'); // insert outside of the container
    $('#container').after('<b>html</b>');

    $('#container').prepend('<b>html</b>'); // insert inside of the container
    $('#container').append('<b>html</b>');

    $('#container').append(domObject); // move domObject

    $(...).wrap('<li></li>'); // wrap each elements
    $(...).wrapInner('<a href="#"></a>'); // place new element inside each elements
    $(...).wrapAll('<ol></ol>'); // move all elements into a single container
    $(...).replaceWith('<p>new text</p>');
    $('<p>new text</p>').replaceAll(...);

    $(...).clone().insertBefore('#target');
    $(...).clone(true); // copy event handler and data, too

    $(...).empty(); // remove all elements inside
    $(...).remove(); // remove all elements inside + remove itself too
    $(...).detatch(); // remove all elements inside + remove itself too + preserve jQuery data

sorting table rows

    var rows = $table2.find('tbody > tr').get();

    rows.sort(function(a, b) {
      var keyA = $(a).data('book')[sortKey];
      var keyB = $(b).data('book')[sortKey];
      if (keyA < keyB) return -sortDirection;
      if (keyA > keyB) return sortDirection;
      return 0;
    });

    $.each(rows, function(index, row) {
      $table2.children('tbody').append(row);
    });


{:class="go-to-index"}
[jQuery ABC](index)
