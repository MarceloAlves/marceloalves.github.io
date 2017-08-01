---
layout: post
title:  "Prevent Links From Escaping Full Screen App"
date:   2017-07-31 16:00:00
categories: notes ios pwa safari
banner_image: ""
featured: false
comments: true
---

After adding a site to your homescreen in iOS, Safari does this weird thing where links will end up opening up a new browser window. This snippet of javascript prevents that:

```javascript
$(document).ready(function(){
  if(window.navigator.standalone == true) {
    $('a').click(function(e) {
      window.location = $(this).attr('href');
      return false;
    });
  }
});
```

However when using Bootstrap, dropdown links stop working. Adding something like this seems to solve the issue:

```javascript
if($(e.target).hasClass('dropdown-toggle')) {
  return;
}
```
