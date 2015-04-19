---
layout: post
title: "My First Blog Post"
date: 2015-04-19
header_desc: Here comes the awesome blog
---

My first blog post using Jekyll

{% highlight csharp linenos %}

using System;

public class Example
{
   private const int MAX_RECURSIVE_CALLS = 1000;
   static int ctr = 0;

   public static void Main()
   {
      Example ex = new Example();
      ex.Execute();
      Console.WriteLine("\nThe call counter: {0}", ctr);
   }

   private void Execute()
   {
      ctr++;
      if (ctr % 50 == 0)
         Console.WriteLine("Call number {0} to the Execute method", ctr);

      if (ctr <= MAX_RECURSIVE_CALLS)
         Execute();

      ctr--;
   }
}

{% endhighlight %}
