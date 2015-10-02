---
layout: post
lang: C#
nav_blog: class="selected"
title: Use WebImage (System.Web.Helpers)
comments: true
description : Resize images using <code>WebImage</code>
date: 2015-04-19
header_desc: WebImage - Resize image on the fly
---

{% highlight csharp linenos %}

namespace Website.AttributeFilters
{
    public class AjaxChildActionOnlyAttribute : ActionMethodSelectorAttribute
    {
        public override bool IsValidForRequest(ControllerContext controllerContext, System.Reflection.MethodInfo methodInfo)
        {
            return controllerContext.RequestContext.HttpContext.Request.IsAjaxRequest() || controllerContext.IsChildAction;
        }
    }
}

{% endhighlight %}
