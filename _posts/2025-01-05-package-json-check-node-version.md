---
layout: post
lang: Node
date: 2025-01-05
title: Verify project.json Node Version
description : How to check for the correct Node version for your project package.json file
comments: true
---

- ./nodeVersion.js

```js
    const VERSION = '20.17.0';
    const nodeVersion = process.versions.node;
    if (nodeVersion) {
        if (nodeVersion !== VERSION) {
            console.log("\x1b[47m\x1b[31m%s\x1b[0m", "-------******* (npm start) failed due to required Node Version: " + VERSION + " *******-------");
            console.log("\x1b[47m\x1b[33m%s\x1b[0m", "-------******* Your current Node Version is: " + nodeVersion + " *******-------");
            process.exit(1);
        }
    } else {
        console.log("\x1b[47m\x1b[31m%s\x1b[0m", "-------******* Something went wrong while checking for Node version *******-------");
        process.exit(1);
    }
```

- ./package.json 

```json
    ....
    "scripts": {
        "preinstall": "node nodeVersion",
        "prestart": "node nodeVersion",
        ....
    }
    ....
```