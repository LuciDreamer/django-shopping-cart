django-shopping-cart
===============

A generic django shopping cart ajax-based solution.

INSTALLATION
------------

Install this package with:

`
python setup.py install
`

Settings
********
Configure the SESSION_SERIALIZER setting to "PickeSerializer", because it can store non-primitive python objects.

```python
SESSION_SERIALIZER = u"django.contrib.sessions.serializers.PickleSerializer"
```

Configure the SESSION_ENGINE setting, it`s recommended to use the 'django.contrib.sessions.backends.cache', or
"signed_cookie", that uses encrypted cookies.

Coonfigure the SHOPPING_CART_PRODUCT_MODEL, to add your product model.
```python
SHOPPING_CART_PRODUCT_MODEL = 'app.Model'
```

Create an configure your template, that will show the shopping cart data.
```python
SHOPPING_CART_TEMPLATE = u'cart.html'
```

This template will receive as context_data, a "cart" object, with the items list.
Each item is an object with the "item_pk", "quantity" and some interesting methods, such as get_object(), that will return the object related with the "item_pk" value.



Simple shopping cart template sample:
```html
<ul>
{% for i in cart.items  %}
    <li>{{ i.quantity  }} do item {{ i.get_object }}</li>
{% endfor %}
</ul>
```

Urls
********

  ```python
  from shopping_cart.urls import shopping_cart_urls
  url(r'^shopping-cart/', include(shopping_cart_urls),),
   ```


How to use:
**********************

This app receives a POST request in the /shopping-cart/ url, with the product_id and the current operation(add, remove).

Example, to add a product on the shopping cart, you could use:

```html
  <form class="cart-manipulation" action="{% url 'shopping-cart-view' %}" method="post">
  {% csrf_token %}
    <input type="hidden" name="item" value="{{ item.pk }}">
  <input type="hidden" name="cart-action" value="add">
  <input type="submit" value="Add to Cart">
  </form>
```

To remove an item from the cart:
```
    <form class="cart-manipulation" action="{% url 'shopping-cart-view' %}" method="post">
  {% csrf_token %}
    <input type="hidden" name="item" value="{{ item.pk }}">
  <input type="hidden" name="cart-action" value="remove">
  <input type="submit" value="Remove from cart">
  </form>
```

This app requires Jquery in the template this cart will be used.

In the end of your template, you must include the following javascript file:
```html
<script type="text/javascript" src="{% static 'shopping_cart/shopping_cart.js' %}"></script>
```

Showing the cart content:
**********************
You just need to create a DIV element, with the id="shopping-cart", and this application will automatically render the template you`ve created and configured in the settings file, on SHOPPING_CART_TEMPLATE setting.
