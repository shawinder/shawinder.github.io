---
layout: post
lang: C#
title: ASP.NET MVC Bundling
comments: true
description : Resolve CSS Image Paths using <code>CssRewriteUrlTransform()</code>
date: 2015-04-19
header_desc: ASP.NET MVC Bundling - Resolve Image Path
---
1 . Add `CssRewriteUrlTransform()` to the `StyleBundle` and use method chaining for multiple includes. Make sure that the includes without `CssRewriteUrlTransform()` should come last in the include list.

```
bundles.Add(new StyleBundle("~/bundles/site1/css").Include(
"~/Content/Tenant/site1/site.css", new CssRewriteUrlTransform()).Include(
"~/Content/font-awesome.min.css", new CssRewriteUrlTransform()).Include(
"~/Content/bootstrap.min.css", new CssRewriteUrlTransform()).Include(
"~/Content/Tenant/site1/sidebar.css",
"~/Content/Tenant/site1/noise/grain.css"));
```
