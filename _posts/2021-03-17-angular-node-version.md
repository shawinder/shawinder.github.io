---
layout: post
lang: Angular
title: Windows - Switch Angular Node version
comments: true
description : How to switch node versions to support a specific Angular CLI version, which runs only on a specific Node version.
date: 2021-03-179
header_desc: Angular switch node versions | Windows
---

- Download and install `nvm` from https://github.com/coreybutler/nvm-windows/releases

- Download and set a specific node version e.g `10.24.0` using following commands:
```
> nvm nvm install 10.24.0
> nvm use 10.24.0
```

- Verify that Node switch is successful.
```
> node --version
```

- Now Install dependencies using normal npm commands using the selected Node version.
```
npm install
```

- If you are interested to install only `package-lock.json` dependencies then use the following command instead:
```
npm ci
```
