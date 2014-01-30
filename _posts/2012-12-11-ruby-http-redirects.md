---
lyout: post
title: Ruby's Net::Http doesn't like redirects and https
---

I'm currently experimenting with ruby and one thing I needed to do, was retrieving the content of an external website.

My first approach to this was using the `Net::Http.get` method. 
**The problem:** this doesn't follow redirects.

My second approach was a more complicated version that catches a redirect and requests the content again with the redirect url.

[Taken from the ruby docs](http://www.ruby-doc.org/stdlib-1.9.3/libdoc/net/http/rdoc/Net/HTTP.html)
<script src="https://gist.github.com/4258731.js?file=fetch.rb"></script>

**The problem:** If the website redirects to https, it will fail with a [400 HttpBadRequest](http://www.ruby-doc.org/stdlib-1.9.3/libdoc/net/http/rdoc/Net/HTTPBadRequest.html). 

## Solution

After a bit of research, I stumbled over the [httpclient](https://rubygems.org/gems/httpclient) gem.

<script src="https://gist.github.com/4258731.js?file=httpclient.rb"></script>

Well, this looks much better!