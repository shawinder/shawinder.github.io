---
layout: post
title: Child Actions - Restrict to Ajax Request Only
comments: true
date: 2015-04-21
header_desc: Child Actions - Restrict to Ajax Request Only
---
<p><span class="step">1</span> Create an attribute filter <code>AjaxChildActionOnlyAttribute</code> and override <code>IsValidForRequest()</code> method.</p>

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
