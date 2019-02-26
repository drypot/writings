---
title: Text, Html, Attr, Prop
---

    $(...).text();
    $(...).text('new text');

    $(...).html();
    $(...).html('new <b>html</b>');

    $(...).val(); // value of form field
    $(...).val('new value');

    $(...).attr('height'); // HTML Attributes
    $(...).attr('height', '100');
    $(...).removeAttr('height');

    $(...).attr({
      rel: 'external',
      id: function(index, oldValue) { // value callback. invoked once per element in the matched set.
        return 'wikilink-' + index;
      }
    });

    // HTML attributes 와 DOM properties 는 이름이나 데이터형이 다를 수 있다.
    // class vs. className, checked='checked' vs. checked: true

    $(...).prop(...); // DOM Properties
    $('.my-checkbox').prop('checked');
    $('.my-checkbox').prop('checked', false);

    $(...).data(key);
    $(...).data(key, value);
    $(...).removeData(key);

    <th data-sort='{"key":"title"}'>Title</th>
    $('th').first().data('sort').key; // the data is stored internally by jQuery and the HTML attribute is no longer accessed


{:class="go-to-index"}
[jQuery ABC](index)
