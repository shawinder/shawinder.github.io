---
layout: post
lang: C#
title: MVC Response - GZip Compression
comments: true
date: 2015-04-21
header_desc: Child Actions - GZip Compression
---
1 . Create an attribute filter `CompressAttribute` and override `OnActionExecuting()` method.</p>

```cs
namespace Website.AttributeFilters
{
    public class CompressAttribute : ActionFilterAttribute
    {
        public override void OnActionExecuting(ActionExecutingContext filterContext)
        {
            // Skip if child action is called, as Response.filter is null for child action.
            if (filterContext.IsChildAction) return;

            HttpRequestBase request = filterContext.HttpContext.Request;

            string acceptEncoding = request.Headers["Accept-Encoding"];

            if (string.IsNullOrEmpty(acceptEncoding)) return;

            acceptEncoding = acceptEncoding.ToUpperInvariant();

            HttpResponseBase response = filterContext.HttpContext.Response;

            if (acceptEncoding.Contains("GZIP"))
            {
                response.AppendHeader("Content-encoding", "gzip");
                response.Filter = new GZipStream(response.Filter, CompressionMode.Compress);
            }
            else if (acceptEncoding.Contains("DEFLATE"))
            {
                response.AppendHeader("Content-encoding", "deflate");
                response.Filter = new DeflateStream(response.Filter, CompressionMode.Compress);
            }
        }
    }
}
```

2 . Register the newly created custom attribute inside `FilterConfig.cs`

```cs
namespace Website
{
    public class FilterConfig
    {
        public static void RegisterGlobalFilters(GlobalFilterCollection filters)
        {
            filters.Add(new AttributeFilters.CompressAttribute());
        }
    }
}
```
