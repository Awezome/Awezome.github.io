---
title: iFrame高度自适应内容
tags:
  - html
id: 2209
categories:
  - Html
date: 2014-10-16 17:20:18
---

```js
function SetWinHeight(obj) {
    var win = obj;
    if (document.getElementById) {
        if (win && !window.opera) {
            if (win.contentDocument && win.contentDocument.body.offsetHeight)
                win.height = win.contentDocument.body.offsetHeight;
            else if (win.Document && win.Document.body.scrollHeight)
                win.height = win.Document.body.scrollHeight;
        }
    }
}

<iframe src="backtop.html" frameborder="0" scrolling="no" id="external-frame" onload="SetWinHeight(this)"></iframe>
```
