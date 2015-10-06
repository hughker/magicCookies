# magicCookies for Shopify
aka. return to collection you added product to cart from
- Uses [JavaScript Cookie](https://github.com/js-cookie/js-cookie)
- Sets `magicCookies` as the _`.noConflict`_ namespace (see [here](https://github.com/js-cookie/js-cookie#namespace-conflicts))

## What does it do?
If you'd like to have your Customer return from their Cart to the Collection page they last visited, this will do the trick.

## Installation
Add `class="continue-shopping"` to relevant links in your `cart.liquid` Template and then add the following include at the bottom of your `theme.liquid` Template:
```html
{% include 'magicCookies' %}
```

---

Here's a peek at the code inside the Snippet:
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
      var magicCookiesTargetLinks = document.getElementsByClassName('continue-shopping');
      for ( var i in magicCookiesTargetLinks )
        if ( magicCookiesTargetLinks[i].className && magicCookiesTargetLinks[i].className.indexOf('continue-shopping') != -1 )
          magicCookiesTargetLinks[i].href = ("\/collections\/") + magicCookiesCollecton;
    </script>
{% endcase %}
```

Note, you can also use CDNJS if you'd rather not host the `js.cookie.min.js` file along with your theme:

Replace:
```html
{{ 'js.cookie.min.js' | asset_url | script_tag }}
```

With:
```html
{{ 'https://cdnjs.cloudflare.com/ajax/libs/js-cookie/2.0.3/js.cookie.min.js' | script_tag }}
```

## Dependencies
* [JavaScript Cookie v2.0.3](https://github.com/js-cookie/js-cookie)

## Contributing
Have something you want to add or remove? Please feel free to submit a pull request, I'd be more than happy to review and accept.

## License
MIT. © 2015 Winston Hughes
