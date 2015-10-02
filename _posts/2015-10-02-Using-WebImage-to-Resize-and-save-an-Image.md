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

public static void ImageResize(string imgPath, int s = 0, int w = 0, int h = 0)
{
    var path = System.IO.Path.IsPathRooted(imgPath) ? imgPath : HttpContext.Current.Server.MapPath(imgPath);
    System.Drawing.Image img = System.Drawing.Image.FromFile(path);
    int width, height = 0;
    if (w == 0 && h == 0)
    {
        width = s == 0 ? img.Width : s;
        height = s == 0 ? img.Height : s;
        new WebImage(path).Resize(width, height, preserveAspectRatio: true, preventEnlarge: true) // Resizing the image to 100x100 px on the fly... 
            //.Crop(1, 1) // **ONLY IF using preserveAspectRation : false - Cropping it to remove 1px border at top and left sides
            //.AddTextWatermark("©UC", fontColor: "red", fontSize: 10, fontStyle: "Bold", opacity: 50, padding: 0)
        .Write();
    }
    else
    {
        width = w == 0 ? img.Width : w;
        height = h == 0 ? img.Height : h;
        new WebImage(path).Resize(width, height, preserveAspectRatio: false, preventEnlarge: true) // Resizing the image to 100x100 px on the fly... 
            .Crop(1, 1) // **ONLY IF using preserveAspectRation : false - Cropping it to remove 1px border at top and left sides
            //.AddTextWatermark("©UC", fontColor: "red", fontSize: 10, fontStyle: "Bold", opacity: 50, padding: 0)
        .Write();
    }
}
{% endhighlight %}
