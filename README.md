# load-more-button-collection

## HTML OF BUTTON

<!-- {%- if paginate.pages > 1 -%}
              {% render 'pagination', paginate: paginate, anchor: '' %}
            {%- endif -%} -->
            {%- if paginate.pages > 1 and paginate.next -%}
 <button id="load-more" data-next-url="{{ paginate.next.url }}">Load More</button>
{%- endif -%}




## JS
<script>
  $(document).ready(function () {
    $('#ProductGridContainer').on('click', '#load-more', function () {
      var button = $(this);
      var nextUrl = button.data('next-url');

      // Append ?view=ajax if not already present
      if (!nextUrl.includes('view=')) {
        nextUrl += (nextUrl.includes('?') ? '&' : '?') + 'view=ajax';
      }

      button.text('Loading...');

      $.get(nextUrl, function (data) {
        var newItems = $(data).find('#product-grid').html();
        $('#product-grid').append(newItems);

        var newButton = $(data).find('#load-more');
        if (newButton.length) {
          button.data('next-url', newButton.data('next-url'));
          button.text('Load More');
        } else {
          button.remove(); // Remove if no more pages
        }
      });
    });
  });
</script>
