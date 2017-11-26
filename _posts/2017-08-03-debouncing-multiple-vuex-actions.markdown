--- 
wordpress_id: PSD-23
layout: post
title: debouncing multiple vuex actions
---

I've recently been using [vuejs](http://vuejs.org) at work. It was a choice between React/Vue as I wanted to be able to have some complexish
JavaScript behavior on just a few pages, not a fully blown Single Page App (SPA). For this particular client, they didn't want an explicit save button on the form,
instead preferring to autosave. So the question became, how do you make sure you can autosave multiple different components (and therefore endpoints) without stepping
on the toes of each save. That is, you can't debounce a (global) save endpoint, you need to have multiple debounced endpoints.

I found this [very helpful Stack Overflow](https://stackoverflow.com/questions/28787436/debounce-a-function-with-argument) which was the solution I needed.

```javascript
  function save (payload) {
    console.log("saving", payload);
  }

  var saveDebounced = _.wrap(_.memoize(function () {
    return _.debounce(save, 5000);
  }, _.property("partId")), function (func, payload) {
    return func(payload)(payload);
  });

  actions: {
    "UPDATE_PART_PROPS": ({ commit }, payload) => {
      commit("UPDATE_PART_PROPERTIES", payload);
      saveDebounced(payload);
    }
  },
```
