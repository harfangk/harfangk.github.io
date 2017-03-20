---
layout: post
ref: where-to-find-plug-errors
date: 2017-02-25 00:00:00 +0900
title: Where to Find Plug Errors
lang: en
---

This post is aimed at those who are just starting to use `Plug`, a great web middleware for Elixir language.

## TL;DR

If you want to access various error messages from `Plug` that do not show up in `Logger`, `use Plug.Debugger` will make those failed requests appear. Check out [Plug.Debugger](https://hexdocs.pm/plug/Plug.Debugger.html#content).

## 404 Error Message Not Found

While building my toy app that received JSON through HTTP POST requests, I noticed an interesting behavior. When I send a request with malformed JSON payload, the request would magically banish - without any trace in logger or server.

So when I send a request with malformed JSON like:

{% highlight json %}
{
  "
}
{% endhighlight %}

The server would respond with `500 Internal Server Error` without any message. It logged no error whatsoever, either.

After some digging, I found out that the sort of error I was looking for - `Plug.Parsers.ParseError` - is not logged by default. That makes sense - I don't want my log to be flooded with error messages about bad requests.

The solution is to use `Plug.Debugger` in the module that uses `Plug.Router`.

The following is part of the code for web interface in my toy app. Notice the `use Plug.Debugger` I included here. 

{% highlight elixir %}
defmodule UrlShortener.Web do
  use Plug.Router
  use Plug.Debugger

  plug Plug.Parsers, parsers: [:json],
                     pass: ["application/vnd.api+json"],
                     json_decoder: Poison

  plug :match
  plug :dispatch

  get "/" do
    conn
    |> send_resp(200, "Url shortener microservice app. Refer to https://github.com/harfangk/url_shortener for further information.")
  end

  ...
  
end
{% endhighlight %}

After calling that macro, your server now internally logs the error message. 

{% highlight elixir %}
02:58:25.008 [debug] ** (Plug.Parsers.ParseError) malformed request, a Poison.SyntaxError exception was raised with message "Unexpected end of input"
    (plug) lib/plug/parsers/json.ex:54: Plug.Parsers.JSON.decode/2
    (plug) lib/plug/parsers.ex:210: Plug.Parsers.reduce/6
    (url_shortener) lib/url_shortener/web.ex:1: UrlShortener.Web.plug_builder_call/2
    (url_shortener) lib/plug/debugger.ex:123: UrlShortener.Web.call/2
    (plug) lib/plug/adapters/cowboy/handler.ex:15: Plug.Adapters.Cowboy.Handler.upgrade/4
    (cowboy) /Users/bonghyunkim/Documents/Projects/url_shortener/deps/cowboy/src/cowboy_protocol.erl:442: :cowboy_protocol.execute/4
{% endhighlight %}

It also responds to the HTTP request with an error message, so that could be another way for debugging your app.