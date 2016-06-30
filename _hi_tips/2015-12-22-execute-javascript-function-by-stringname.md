---
layout: post
title: Execute javascript function by string(name)
date: '2015-12-22T06:01:21+08:00'
tags:
- javascript
- metaprogramming
- function
- execution
tumblr_url: http://hi-tips.tumblr.com/post/135708645846/execute-javascript-function-by-stringname
---

```js
function executeFunctionByName(functionName, context /*, args */) {
    var args = Array.prototype.slice.call(arguments, 2);
    var namespaces = functionName.split(".");
    var func = namespaces.pop();
    for (var i = 0; i < namespaces.length; i++) {
        context = context[namespaces[i]];
    }
    return context[func].apply(context, args);
}
```


via http://stackoverflow.com/questions/359788/how-to-execute-a-javascript-function-when-i-have-its-name-as-a-string
