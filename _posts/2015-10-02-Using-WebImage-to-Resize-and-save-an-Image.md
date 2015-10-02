---
layout: post
title: WebImage - Resize image on the fly
comments: true
date: 2015-10-02
header_desc: Using WebImage (System.Web.Helpers)
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
