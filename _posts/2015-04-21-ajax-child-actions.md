---
layout: post
lang: C#
title: Child Actions - Restrict to Ajax Request Only
comments: true
date: 2015-04-21
header_desc: Child Actions - Restrict to Ajax Request Only
---
1 . Create an attribute filter `AjaxChildActionOnlyAttribute` and override `IsValidForRequest()` method.

```cs
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
```

2 . Add the newly created custom attribute to the child action.

```cs
[AttributeFilters.AjaxChildActionOnly]
public PartialViewResult AsyncWidget()
{
    return PartialView("_AsyncWidget");
}
```
