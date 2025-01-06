---
layout: post
lang: Typescript
date: 2025-01-04
title: Typescript Enum Alternative
description : A Type can be better used instead of an Enum in Typescript
comments: true
---

Using Enum

```typescript
    enum LoginMode {
        email= 'email',
        social= 'social'
    };

    function initiateLogin(mode: LoginMode) {
        //..
    }

    initiateLogin(LoginMode.email); // OK

    initiateLogin('social'); // Error: Argument of type 'string' is not assignable to parameter of type 'LoginMode'.
```

Using Type (Enum Alternative)

```typescript
    const LoginMode = {
        email: 'email',
        social: 'social',
    };

    type LoginMode = typeof LoginMode[keyof typeof LoginMode];

    function initiateLogin(mode: LoginMode) {
        //..
    }

    initiateLogin(LoginMode.email); // OK
    
    initiateLogin('social'); // OK
```