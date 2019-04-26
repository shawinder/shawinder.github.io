---
layout: post
lang: C#
title: Httpclient could not create ssl/tls secure channel
comments: true
description : Httpclient could not create ssl/tls secure channel
date: 2017-08-03
header_desc: Httpclient could not create ssl/tls secure channel
---

## Simply add following line before `httpclient` initialization code:

```cs
ServicePointManager.SecurityProtocol = (SecurityProtocolType)768 | (SecurityProtocolType)3072;
```

## Complete code will look something like this:
```cs
ServicePointManager.SecurityProtocol = (SecurityProtocolType)768 | (SecurityProtocolType)3072;
using (var httpClient = new HttpClient())
{
    httpClient.BaseAddress = new Uri(Constant.Endpoint.Event.Base);
    var access_token = GetAccessToken();
    httpClient.DefaultRequestHeaders.TryAddWithoutValidation("Authorization", string.Format("Bearer {0}", access_token));

    var stringPayload = JsonConvert.SerializeObject(data);
    var httpContent = new StringContent(stringPayload, Encoding.UTF8, "application/json");

    var endpoint = GetEndpoint(eventType);
    var resp = httpClient.PostAsync(endpoint, httpContent).Result;
    var content = resp.Content.ReadAsStringAsync().Result;
}
```
