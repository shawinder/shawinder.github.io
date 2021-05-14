---
layout: post
lang: AWS
title: Visual Studio AWS Serverless.Template CORS
comments: true
description : How to enable CORS for Visual Studio AWS API Gateway | SAM Serverless template.
date: 2021-05-14
header_desc: Visual Studio AWS Serverless.Template CORS
---

- Simply add the new `Globals` section as below. You can change `*` to appropriate request Url. 

```shell
{
  "AWSTemplateFormatVersion" : "2010-09-09",
  "Transform" : "AWS::Serverless-2016-10-31",
  "Description" : "An AWS Serverless API Application",
  "Globals" : {
    "Api" : {
      "Cors" : {
        "AllowMethods": "'*'",
        "AllowHeaders": "'*'",
        "AllowOrigin": "'*'"
      }
    }
  }
  ....
```
