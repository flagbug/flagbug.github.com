---
layout: post
title: Link Github issues with Javascript
---

Here is a quick code snippet that I used in the new [Mahapps.Metro News page](http://mahapps.com/MahApps.Metro/news) to link Github issue numbers to their corresponding URL:

```js
$(function() {
  $(".yourclass").each(function(element) {
    var replaced = $(this).html().replace(/(#([0-9]+))/g, '<a href="https://github.com/<your-repo-here>/issues/$2">$1</a>');
    $(this).html(replaced);
  });
})
```

Since we're keeping our release notes in our repository and just copy pasted it into the Github releases section, this is a neat way to avoid linking each issue manually.