---
title: react hook的基本使用
---

#### 1.useState

useState可以让函数组件拥有自己的一个状态,和class组件一样去控制组件内部数据的一个状态

下面是一个用按钮通过useState设置初始状态,根据点击事件来改变这个状态

```js
import React,{useState} from 'react'

export default function App() {
    
    const [state, setstate] = useState({
        num:0,
    })    

    const AddClickAction = ()=>{
        setstate({
            num:state.num+1
        })
        console.log(state);
    }

    return (
        <div className="app">
        //点击的时候增加
            <button onClick={AddClickAction}>点我:{state.num}</button>
        </div>
    )
}
```

#### 2.useRef

1. 可以用来获取节点dom(节点标签)

```js
import React,{useRef} from 'react'

export default function App() {

    const ref = useRef()

    // 获取到button按钮的节点
    const getNode = ()=>{
        console.log(ref.current);
    }

    return (
        <div className="app">
            <button onClick={getNode} ref={ref}>点我</button>
        </div>
    )
}
```

2. useRef的current属性是对原来的一个引用,可以用它来做判断,current属性值发生改变的时候他不会去重新走render,这个下面的useEffect模拟生命周期会有写到

#### 3.useEffect

- useEffect有俩个参数:

1. 第一个参数是一个函数,里面可以去写一些数据操作或业务逻辑一些
2. 第二个参数是对上一个函数的依赖,[] 空依赖则只有页面第一次加载的时候才会去执行,[有依赖的参数] 如果依赖函数中操作的某个数据,当依赖的数据发生改变的时候会去执行useEffect里面被依赖的代码,如果不写第二个参数,当页面重新走render会执行函数内部的所有代码

下面是对空依赖和不写依赖

```js
import React,{useEffect,useState} from 'react'

export default function App() {

    const [state, setstate] = useState(0);

    // 我只会执行一次
    useEffect(() => {
        console.log(state,"执行一次"); //state 0
    }, [])

    // 页面加载执行一次,当每次点击改变state数据的时候会继续执行
    // 如果第二个参数不写的话函数内部执行的逻辑都会执行
    useEffect(() => {
        console.log(state,"改变state就会执行一次");
    }, [state])

    const AddClickAction = ()=>{
        setstate(state+1);
    }

    return (
        <div className="app">
            <button onClick={AddClickAction}>点我:{state}</button>
        </div>
    )
}
```



- useEffect可以模拟类组件的一个生命周期

```js
import React,{useEffect,useState,useRef} from 'react'

export default function App() {

    const [state, setstate] = useState(0);
    const ref = useRef(true);
    // 页面加载时执行...  componentDidMount
    useEffect(() => {
        console.log("页面结构已经创建完成.....");
    }, [])

    // 状态改变或者时走render的时候,组件重新加载会执行... componentDidUpDate
    // useEffect(() => {
    //     console.log("state改变...");   
    // })
    // 但是你会发现在页面加载的时候会执行componentDidMount和componentDidUpDate,这并不是我们所期望的.
    // 通过useRef来达到componentDidUpDate的一个效果
    // 通过判断ref的current的值 你会发现页面加载的时候不回去执行下面这个useEffect 只有当数据发生改变或者        重走render的时候才回去执行
    useEffect(() => {
        if(ref.current){
            ref.current = false;
            return;
        }
        console.log("state改变...");   
    })

    // useEffect内部return的函数 只有当组件销毁的时候才会去执行... componentWillUnmount
    useEffect(() => {
        return () => {
            console.log("销毁....");
        }
    },[])

    const AddClickAction = ()=>{
        setstate(state+1);
    }

    return (
        <div className="app">
            <button onClick={AddClickAction}>点我:{state}</button>
        </div>
    )
}
```

#### 4.useCallback

可以提高react的一个优化,也可以减少事件的创建,同时可以缓存函数

1. 没有用useCallback,下面这个每次点击改变state的时候都会走render,同时点击事件也会被重新创建,每次点击都会去打印 增加后的state数据

```js
import React,{useState} from 'react'

export default function App() {

    const [state, setstate] = useState(0)

    return (
        <div className="app">
            <button onClick={()=>{
                setstate(state+2)
                console.log(state);
            }}>点我:{state}</button>
        </div>
    )
}
```

2. 用useCallback,addAction事件被useCallback缓存了下来,每次点击的时候打印的都是一开始的数据

```js
import React,{useCallback, useState} from 'react'

export default function App() {

    const [state, setstate] = useState(0)

    const addAction = useCallback(()=>{
        setstate(state+2)
        console.log(state);
    },[])

    return (
        <div className="app">
            <button onClick={addAction}>点我:{state}</button>
        </div>
    )
}
```

#### 5.useMemo

对一个值进行一个缓存

在计算或者是对数据进行操作的时候,不会因为组件的更新或者是整个组件的重新渲染再去计算或者是操作这个数据,用useMemo会在依赖的数据发生变化的时候就会去计算

```js
import React,{ useState,useMemo,useCallback} from 'react'

export default function App() {

    const [state, setstate] = useState(0)

    const fnc =useCallback(() => {
        setstate(state+1)
    },[state]) 

    // 当fnc函数内部的数据发生变化会去计算state的数据
    const addAction = useMemo(()=>{
        return  fnc;
    },[fnc])

    return (
        <div className="app">
            <button onClick={addAction}>点我:{state}</button>
        </div>
    )
}
```

#### 6.memo

memo相当于shouldComponentUpdate和PureComponent对性能的优化,

他是减少没必要的组件渲染

下面代码 每次点击修改state数据的时候都会重新渲染重新走one子组件,但是子组件内部没有数据要修改,这样每次渲染都会去执行one组件,这种没必要的渲染需要避免,

```js
#App 父组件
import React,{ useState,useMemo,useCallback} from 'react'
import One from './One'
export default function App() {

    const [state, setstate] = useState(0)

    const fnc =useCallback(() => {
        setstate(state+1)
    },[state]) 

    const addAction = useMemo(()=>{
        return  fnc;
    },[fnc])

    return (
        <div className="app">
            <button onClick={addAction}>点我:{state}</button>
			// one组件
            <One/>
        </div>
    )
}

```

打印的结果

![image-20210801224311088](C:\Users\asus\AppData\Roaming\Typora\typora-user-images\image-20210801224311088.png)

在子组件内部用memo 包裹函数,这样父组件渲染,子组件不会去执行,只有当子组件内部修改了数据才会去执行

```js
#one 子组件
import React,{memo} from 'react'
// 用memo 减少无用组件的渲染
export default memo(function One() {
    console.log("我是one组件...");
    return (
        <div>
            我是one组件
        </div>
    )
}) 
```

小总结: 我对hook的理解也只是在这简单的使用层面上,以后也会去更深入的去学习,去使用,可能也会有些没有说到的,没有去使用到的,告诉我一下,我也去多学习学习!!!







