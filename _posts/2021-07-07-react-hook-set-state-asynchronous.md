---
layout: post
title: Handle setState asynchronous in React Hook
subtitle: Fetching asynchronous data problem
cover-img: /assets/img/path.jpg
thumbnail-img: /assets/img/thumbnail/1.jpg
share-img: /assets/img/path.jpg
gh-repo: https://github.com/hadomanh/
gh-badge: [star, fork, follow]
tags: [React, TIL]
---

## Context
- Sử dụng Functional Component
- Hàm **getInstance** là bất đồng bộ, dùng để khởi tạo instance
- Trong Component, gọi lần lượt 2 hàm sau:
    - Hàm **loadInstance** khởi tạo *instance* trong Component, lưu trực tiếp vào trong state với hàm **setInstance**
    - Hàm **useInstance** sẽ lấy instance từ trong state và sử dụng 

```js
import React, { useState, useEffect } from 'react'
import getInstance from './utils/getInstance'

function App () {
    const [instance, setInstance] = useState(null)

    useEffect(() => {
        async function fetchData () {
            await loadInstance()
            await useInstance()
        }
        fetchData()
    }, [])

    const loadInstance = async () => {
        const initialInstance = await getInstance()
        setInstance(initialInstance)
    }

    const useInstance = async () => {
        // Do something with *instance*
        const value = await instance.getValue()
        doSomething(value)
    }

    // Something else...
}
```

## Problem
- Mặc dù 2 hàm **loadInstance** và **useInstance** được thực hiện tuần tự, đúng với logic ban đầu, nhưng khi hàm **useInstance** lấy *instance* trong state để sử dụng, *instance* vẫn mang giá trị **null**

## Cause
- Hàm setState của React Hook (ở đây là **setInstance**) là bất đồng bộ
- Mặc dù được thực hiện setState trước trong hàm **loadInstance**, nhưng khi hàm **useInstance** sử dụng, giá trị khởi tạo vẫn chưa kịp lưu vào trong state. Khi đó, state này vẫn sử dụng giá trị mặc định là **null**
- Không giống như **setState** trong Class Component, hàm setter của **useEffect** không cung cấp callback để xử lý sau khi update

## Solution
- Cách 1: Gộp 2 hàm, sử dụng trực tiếp instance sau khi load, không sử dụng state

```js
import React, { useState, useEffect } from 'react'
import getInstance from './utils/getInstance'

function App () {
    const [instance, setInstance] = useState(null)

    useEffect(() => {
        useInstance()
    }, [])

    const useInstance = async () => {
        // Load instance
        const initialInstance = await getInstance()
        setInstance(initialInstance)

        // Do something with *initialInstance*
        const value = await initialInstance.getValue()
        doSomething(value)
    }

    // Something else...
}
```

- Cách 2:

```js
import React, { useState, useEffect } from 'react'
import getInstance from './utils/getInstance'

function App () {
    const [instance, setInstance] = useState(null)

    useEffect(() => {
        loadInstance()
    }, [])

    // This approach replicate the same behavior with callback for setState
    useEffect(() => {
        if (instance) {
            useInstance()
        }
    }, [instance])

    // Omitted...
}
```
