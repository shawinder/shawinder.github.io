---
layout: post
lang: React
title: React Function Component ForwardRef
comments: true
description : Ant Design Function components cannot be given refs. Attempts to access this ref will fail. Did you mean to use React.forwardRef()?
date: 2015-04-19
header_desc: Function components cannot be given refs. Attempts to access this ref will fail
---

- I have created a following `ant-design` InputNuber Wrapper to add a `suffix`.But React was showing `Warning: Function components cannot be given refs. Attempts to access this ref will fail. Did you mean to use React.forwardRef()?`

```
import React from 'react';
import { InputNumber } from 'antd';

function InputNumberSuffix(props) {
    const { suffix } = props;
    if (suffix) {
        return (
            <span className="input-number-suffix ant-input-affix-wrapper">
                <InputNumber {...props} />
                <span className="ant-input-suffix">{suffix}</span>
            </span>
        );
    }
    return (
        <InputNumber {...props} />
    );
}
export default InputNumberSuffix;
```
- And the `solution` was simply to pass a forward reference using following code:

```
import React from 'react';
import { InputNumber } from 'antd';

const InputNumberSuffix = React.forwardRef((props, ref) => {
    const { forwardedRef, suffix, ...rest } = props;
    if (suffix) {
        return (
            <span className="input-number-suffix ant-input-affix-wrapper">
                <InputNumber ref={ref} {...rest} />
                <span className="ant-input-suffix">{suffix}</span>
            </span>
        );
    }

    return (
        <InputNumber ref={ref} {...rest} />
    );
});
export default InputNumberSuffix;
```

- Another solution is to simply convert your `function component` to a `class component`.
