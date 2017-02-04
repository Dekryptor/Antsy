﻿# Antsy Web Framework

This is a web framework for people who just want to get going. This is a minimalist framework, 
most of the 'enterprise' features have been sacrificed in order to have a simple api. 

As a demonstration, here's the obligatory hello world:
```csharp
using Antsy;

class Program
{
    static void Main(string[] args)
    {
        var host = new AntsyHost(port: 80);
        host.Get("/hello", (req, res) =>
        {
            res.Text("hello world");
        });
        host.Run();
    }
}
```

Antsy wraps [ASP.NET Core](https://www.asp.net/core) so that it doesn't reinvent the wheel,
but the api surface is inspired by [Express JS](http://expressjs.com/).

The name is a bit of a rib at [Nancy](http://nancyfx.org/). If this actually becomes popular, my sincerest appologies
to the Nancy maintainers. Any name confusion was both entirely avoidable, and my fault.

This a version 1.0 library. Because it leans heavily on aspnet this framework 
should theoretically be relatively robust, but no guarantees. Note that the host will not listen for https
connections (Just stick your favorite reverse proxy in front and do ssl termination there).


Nuget:
```
install-package microsoft.aspnetcore.http.abstractions
install-package antsy
```

### API

##### Constructor

```csharp
new AntsyHost(int port = 80);
```

##### Methods (AntsyHost)

The path variable should follow asp.net [routing rules](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/routing).

```csharp
//Async or non-async, your choice.
host.Get(string path, Func<AntsyRequest, AntsyResponse, Task>);
host.Get(string path, Action<AntsyRequest, AntsyResponse>);
host.Post(string path, Func<AntsyRequest, AntsyResponse, Task>);
host.Post(string path, Action<AntsyRequest, AntsyResponse>);
host.Delete(string path, Func<AntsyRequest, AntsyResponse, Task>);
host.Delete(string path, Action<AntsyRequest, AntsyResponse>);
//folderRoot is relative to the current directory.
host.StaticFiles(string path, string folderRoot);
```

##### Request & Response

The ```req``` and ```res``` are the standard ASP.NET 
[HttpRequest](https://docs.microsoft.com/en-us/aspnet/core/api/microsoft.aspnetcore.http.httprequest#Microsoft_AspNetCore_Http_HttpRequest)
and 
[HttpResponse](https://docs.microsoft.com/en-us/aspnet/core/api/microsoft.aspnetcore.http.httpresponse#Microsoft_AspNetCore_Http_HttpResponse)
objects,
but gain helper methods to make them line up more with the Express API.

The helper methods on the response object are:
```csharp
//Accepts either a POCO or a string.
res.Json(object obj);
res.Text(string text);
res.Html(string html);
//Serves the file as an attachment.
res.File(string filepath);
res.File(string filename, Stream filestream);
```
These helper methods just make sure the response headers are properly formatted (Content-Type, and friends).

### License

MIT
