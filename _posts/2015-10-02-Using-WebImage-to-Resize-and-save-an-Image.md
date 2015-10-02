---
layout: post
title: WebImage - Resize image on the fly
comments: true
date: 2015-10-02
header_desc: Using WebImage (System.Web.Helpers)
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
        new WebImage(path).Resize(width, height, preserveAspectRatio: true, preventEnlarge: true).Write();
    }
    else
    {
        width = w == 0 ? img.Width : w;
        height = h == 0 ? img.Height : h;
        new WebImage(path).Resize(width, height, preserveAspectRatio: false, preventEnlarge: true)
            .Crop(1, 1) // **ONLY IF using preserveAspectRation : false - Cropping it to remove 1px border at top and left sides
            //.AddTextWatermark("©UC", fontColor: "red", fontSize: 10, fontStyle: "Bold", opacity: 50, padding: 0)
        .Write();
    }
}

{% endhighlight %}
