# magicCookies for Shopify
aka. return to collection you added product to cart from
- Uses [JavaScript Cookie](https://github.com/js-cookie/js-cookie)
- Sets `magicCookies` as the _`.noConflict`_ namespace (see [here](https://github.com/js-cookie/js-cookie#namespace-conflicts))
- Rewrites all `/collections/all` links to `/collections/PreviousCollectionHandle` (see [Caveats](#caveats))

## What does it do?
If you'd like to have your customer return from your cart to the collection page they added a product from, this should do the trick.

## Installation
All you have to do is include the following code at the bottom of your `theme.liquid` for example:
```html
{% include 'magicCookies' %}
```

---

Here's a peak of the code inside the snippet:
```html
{% case template %}
  {% when 'collection' %}
    {{ 'js.cookie.min.js' | asset_url | script_tag }}
    <script type="text/javascript">
      var magicCookies = Cookies.noConflict();
      magicCookies.set('collection', '{{ collection.handle }}');
    </script>
  {% when 'cart' %}
    {{ 'js.cookie.min.js' | asset_url | script_tag }}
    <script type="text/javascript">
      var magicCookies = Cookies.noConflict();
      var magicCookiesCollecton = magicCookies.get('collection');
      $('a').each(function() {
        var value = $(this).attr('href');
        if (value == '\/collections\/all') {
          $(this).attr('href', '\/collections\/' + [magicCookiesCollecton]);
        }
      });
    </script>
{% endcase %}
```

Note, you can also use CDNJS if you'd rather not upload the `js.cookie.min.js` file to your theme:

Replace:
```html
{{ 'js.cookie.min.js' | asset_url | script_tag }}
```

With:
```html
{{ 'https://cdnjs.cloudflare.com/ajax/libs/js-cookie/2.0.3/js.cookie.min.js' | script_tag }}
```

In the `snippets/magicCookies.liquid` file and you'll be all set, if you're adding it via a snippet that it.

## Caveats
As the script stands right now, it'll replace all links that match `/collections/all` with `/collections/PreviousCollectionHandle`. I'm working on a more universal fix, but until then, just keep that in mind as you might have to tweak the JavaScript a bit.

## Contributing
Have something you want to add or remove? Please feel free to submit a pull request, I'd be more than happy to review and accept.

## License
MIT. © 2015 Winston Hughes
