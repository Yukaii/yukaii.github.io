---
layout: post
title: Rewrite cookie cross subdomain using nginx
---

Imagine this case. Running an application in the root domain `https://hello.world`, and we want to share cookie across subdomain(like oh.hello.world). The easiest way come to mind is modifying the `Domain` value in `Set-Cookie` header.

Change

```txt
Set-Cookie: xxx.id=123nfasdf;lkjn; Domain=hello.world
```

to

```txt
Set-Cookie: xxx.id=123nfasdf;lkjn; Domain=.hello.world
```

---

By `express` or any other modern web framework, this change could be done in the application layer(via some cookie/session configuration). However in my case, I can't simply change cookie domain due to some **legacy package problem**. (I JUST CANT find out why...)

Since the app runs behind nginx reverse proxy, I do some googling and found that there is a built-in module for solving these kind of problem in nginx.

### proxy_cookie_domain

Here are the patterns I've tried(but none of them works):

```nginx
proxy_cookie_domain hello.world ".hello.world";
proxy_cookie_domain hello.world .hello.world;
proxy_cookie_domain * .hello.world;
proxy_cookie_domain localhost .hello.world;
```

And finally this works:

```nginx
proxy_cookie_domain ~^(.+)(Domain=hello.world)(.+)$ "$1 Domain=.hello.world $3";
```

The regex split original cookie and rewrite the middle part(domain part).

### Why?

I think it's related to the complex of current cookie. The above example not match server cookie, so I need to specify the regex pattern explicitly.

That's it :p
