---
title: Plugin
---

create jQuery namespace

    (function($) {
      ...
    })(jQuery);

Single function plugin

    $.myPlugin = function () { ... };

Iterator plugin

    $.fn.myFunc = function(opts) {
      var options = $.extend({}, $.fn.myFunc.defaults, opts);
      return this.each(function () {
        $(this).css(...);
      };
    };

    $.fn.myFunc.defaults = {
      ...
    };

Traversal plugin

    $.fn.column = function() {
      var $cells = $();
      this.each(function() {
        var $td = $(this).closest('td, th');
        if ($td.length) {
          var colNum = $td[0].cellIndex + 1;
          var $columnCells = $td
                .closest('table')
                .find('td, th')
                .filter(':nth-child(' + colNum + ')');
          $cells = $cells.add($columnCells);
        }
      });
      return this.pushStack($cells);
    };

Subselector plugin

    $.extend(jQuery.expr[':'], {
      filterName: function (element, index, matches, set) {
        return true;
      }
    });


{:class="go-to-index"}
[jQuery ABC](index)
