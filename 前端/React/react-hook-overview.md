##### 什么是HOOK?

> Hook是React16.8新增的特性。它可以让你在不编写class的情况下使用state以及其他react特性

##### useState钩子

```jsx
import React, { useState } from 'react'

const useStateExmple = () => {
    const [count, setCount] = useState(0)
    
    return (
        <div>
            <button onClick={() => setCount(count + 1)}>click me</button>
        </div>
    )
}
```

useState钩子唯一的参数就是初始state，这个初始state参数只有在第一次渲染时会被调用到。

##### useEffect钩子

useEffect是一个Effect Hook，给函数组件增加了操作副作用的能力。它跟class组件中compmentDidMount,componentDidUpdate和componentWillUnmount具有相同的用途，只不过被合成了一个API

```jsx
import React, { useState, useEffect } from 'react'

const effect = () => {
    const [count, setCount] = useState(0)
    
    useEffect(() => {
        document.title = `You clicked ${count} times`
    })
    
    return (
        <button onClick={() => setCount(count + 1)}>click me</button>
    )
}
```





---

that's all