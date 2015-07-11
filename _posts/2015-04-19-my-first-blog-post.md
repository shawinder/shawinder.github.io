---
layout: post
lang: c#
nav_blog: class="selected"
title: ASP.NET MVC Bundling
description : Resolve CSS Image Paths using <code>CssRewriteUrlTransform()</code>
date: 2015-04-19
header_desc: ASP.NET MVC Bundling - Resolve Image Path
---
<p><span class="step">1</span> Add <code>CssRewriteUrlTransform()</code> to the <code>StyleBundle</code> and use method chaining for multiple includes. Make sure that the includes without <code>CssRewriteUrlTransform()</code> should come last in the include list.</p>

{% highlight csharp linenos %}

bundles.Add(new StyleBundle("~/bundles/site1/css").Include(
"~/Content/Tenant/site1/site.css", new CssRewriteUrlTransform()).Include(
"~/Content/font-awesome.min.css", new CssRewriteUrlTransform()).Include(
"~/Content/bootstrap.min.css", new CssRewriteUrlTransform()).Include(
"~/Content/Tenant/site1/sidebar.css",
"~/Content/Tenant/site1/noise/grain.css"));

{% endhighlight %}

{% include disqus.html %}
