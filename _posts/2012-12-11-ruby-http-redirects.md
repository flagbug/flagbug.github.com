---
lyout: post
title: Ruby's Net::Http doesn't like redirects and https
---

I'm currently experimenting with ruby and one thing I needed to do, was retrieving the content of an external website.

My first approach to this was using the `Net::Http.get` method. 
**The problem:** this doesn't follow redirects.

My second approach was a more complicated version that catches a redirect and requests the content again with the redirect url.

[Taken from the ruby docs](http://www.ruby-doc.org/stdlib-1.9.3/libdoc/net/http/rdoc/Net/HTTP.html)

```rb
require 'net/http'
require 'uri'
 
def fetch(uri_str, limit = 10)
  # You should choose a better exception.
  raise ArgumentError, 'too many HTTP redirects' if limit == 0
 
  response = Net::HTTP.get_response(URI(uri_str))
 
  case response
  when Net::HTTPSuccess then
    response
  when Net::HTTPRedirection then
    location = response['location']
    warn "redirected to #{location}"
    fetch(location, limit - 1)
  else
    response.value
  end
end
```

**The problem:** If the website redirects to https, it will fail with a [400 HttpBadRequest](http://www.ruby-doc.org/stdlib-1.9.3/libdoc/net/http/rdoc/Net/HTTPBadRequest.html). 

## Solution

After a bit of research, I stumbled over the [httpclient](https://rubygems.org/gems/httpclient) gem.

```rb
require 'httpclient'
 
client = HTTPClient.new
content = client.get_content("http://example.com")
```

Well, this looks much better!
