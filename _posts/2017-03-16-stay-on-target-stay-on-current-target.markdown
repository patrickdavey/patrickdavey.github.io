--- 
wordpress_id: PSD-16
layout: post
title: stay on target. stay on current-target
---

I was setting up an event handler in jQuery today. I want to move to React or Vue, but just not there yet.

Anyway, the JavaScript was falling over because I had set some data-attributes on the element, and then
I was looking for those elements in the callback.

Unfortunately, I was looking at the `target` instead of the `currentTarget`. Basically if you have
HTML like this:

```html
  <button class="outer" data-blah="something">
    <i class="inner">something</i>
  </button>
```

```javascript
  $(document).on("click", ".outer", callSomeFunction);
```


Then, depending on whether you click on the inner or the outer element, the `event.target` will point to the
inner or outer element (i.e what you actually clicked on, not what you've bound the delegated event to)

However, if you use `event.currentTarget` all will be well in the world.
