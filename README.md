# React.js

# React Hooks
> Hooks allow function components to have access to state and other React features(like life cycle methods). Because of this, class components are generally no longer needed.
### Hook Rules
```
There are 3 rules for hooks:

Hooks can only be called inside React function components.
Hooks can only be called at the top level of a component.
Hooks cannot be conditional
```
## 1. useState
> ### The React useState Hook allows us to track state in a function component.
```
<!-- paste directly in sandbox -->
<!-- eg1: -->
import React, { useState } from 'react'

const App = () => {
    const [text,setText] = useState('hello');
    const handleText=()=>{
        setText('hello');
    }
  return (
    <div>
      <input onChange={(e)=>setText(e.target.value)} value={text} type="text" />
      <h1>Yor typed here: {text}</h1>
      <button onClick={()=>handleText()}>reset</button>
    </div>
  )
}

export default App;

<!-- eg2 -->
const App = () => {
  const [checked, setChecked] = useState(false);
  const handleChecked = (e) => {
    //  setChecked((prev)=>!prev);
    setChecked(e.target.checked);
  };
  return (
    <div>
      <input type="checkbox" checked={checked} onChange={handleChecked} />
      <h1>you {checked ? "Liked the" : "Don't like the"} post.</h1>
    </div>
  );
};

export default App;

<!-- eg3 -->
Updating objects and arrays in state 
You can put objects and arrays into state. In React, state is considered read-only, so you should replace it rather than mutate your existing objects. For example, if you have a form object in state, don’t mutate it:
// 🚩 Don't mutate an object in state like this:
form.firstName = 'Taylor';

Instead, replace the whole object by creating a new one:

// ✅ Replace state with a new object
setForm({
  ...form,
  firstName: 'Taylor'
});

import React,{useState} from 'react'

const App = () => {
  const [form,setForm]=useState({
    firstname:"Barbara",
    lastname:"hock",
    email:"barbarahock@gmail.com"
  })
  
  return (
    <div className='App' style={{display:"flex", flexDirection:"column", justifyContent:"center",alignItems:"center"}}>
      <label htmlFor="">First Name:
      <input type="text" value={form.firstname} onChange={(e)=>setForm({
        ...form,
        firstname:e.target.value
      })} placeholder='firstname' name="" id="" />
      </label>
      <label htmlFor="">Last Name:
      <input type="text" value={form.lastname} onChange={(e)=>setForm({
        ...form,
        lastname:e.target.value
      })}placeholder='lastname' name="" id="" />
      </label>
      <label htmlFor="">Email:
      <input type="text" value={form.email} onChange={(e)=>setForm({
        ...form,
        email:e.target.value
      })} placeholder='emailname' name="" id="" />
      </label>
      <h1>{form.firstname}{form.lastname} ({form.email})</h1>
    </div>
  )
}

export default App


```
## useContext or Context Api
React Context is a way to manage state globally.

It can be used together with the useState Hook to share state between deeply nested components more easily than with useState alone.
![2y95s9edrgzrd6t343z9](https://github.com/MohdAqib8267/React.js/assets/106628860/299a2843-51b7-4a2d-9c04-a6afd42908ed)

![images](https://github.com/MohdAqib8267/React.js/assets/106628860/3a8b76ba-a777-4dfb-a27d-82dc0945e9d2)

>in Parent comp:
> To create context, you must Import createContext and initialize it:
import { useState, createContext } from "react";

const UserContext = createContext()
> Next we'll use the Context Provider to wrap the tree of components that need the state Context and and supply the state value
> In child comp:
In order to use the Context in a child component, we need to access it using the useContext Hook.
>import { useState, createContext, useContext } from "react";
>  const user = useContext(UserContext);
> Then you can access the user Context in all components:

```
<!-- eg1 [ pass data from parent to childs] -->
<!-- App.js -->
import React,{ createContext,useState }  from 'react';
import Child from "./Components/Child";
import Child2 from './Components/Child2';

export const grlobalInfo = createContext();
function App() {
  const [color,setColor]=useState('green');
  return (
    <grlobalInfo.Provider value={{appColor:color}}>
    <div className='App'>
      <h1>Parent</h1>
      <Child />
      <Child2 />
      <button onClick={()=>setColor('red')}>Red</button>
      <button onClick={()=>setColor('green')}>green</button>
    </div>
    </grlobalInfo.Provider>
  )
}

export default App

<!-- first child of App.js -->

import React,{useContext} from 'react'
import { grlobalInfo } from '../App';
import SuperChild from './SuperChild';
const Child = () => {
    const {appColor}=useContext(grlobalInfo);
    console.log(appColor);
  return (
    <div className='child'>
      <h1 style={{backgroundColor:appColor}}>child component 1</h1>
      <SuperChild />
    </div>
  )
}

export default Child
 
 <!-- Super child of child1 -->
 import React,{useContext} from 'react'
import { grlobalInfo } from '../App'

const SuperChild = () => {
    const {appColor}=useContext(grlobalInfo);
  return (
    <div>
      <h1 style={{background:appColor}}>SuperChild of child1</h1>
    </div>
  )
}

export default SuperChild

<!-- child 2 of App.js -->

import React,{useContext} from 'react'
import { grlobalInfo } from '../App';

const Child2 = () => {
    const {appColor}=useContext(grlobalInfo);
    console.log(appColor);
  return (
    <div className='child'>
      <h1 style={{backgroundColor:appColor}}>child component 2</h1>
      
    </div>
  )
}

export default Child2


<!-- pass Data from child to parent -->

<!-- Here pass day from child to App -->
<!-- App.js -->
export const grlobalInfo = createContext();
function App() {
  const [color,setColor]=useState('green');
  const [day,setDay]=useState("Monday")
  const getDay=(item)=>{
    // console.log(item);
    setDay(item);
  }
  return (
    <grlobalInfo.Provider value={{appColor:color,getDay:getDay}}>
    <div className='App'>
      <h1>Parent {day}</h1>
      <Child />
      <Child2 />
      <button onClick={()=>setColor('red')}>Red</button>
      <button onClick={()=>setColor('green')}>green</button>
    </div>
    </grlobalInfo.Provider>
  )
}

export default App

<!-- child.js -->
import React,{useContext} from 'react'
import { grlobalInfo } from '../App';
import SuperChild from './SuperChild';
const Child = () => {
    const {appColor,getDay}=useContext(grlobalInfo);
    const day="sunday";
    console.log(appColor);
  return (
    <div className='child'>
      <h1 style={{backgroundColor:appColor}}>child component 1</h1>
      <button onClick={()=>getDay(day)}>click me</button>
      <SuperChild />
    </div>
  )
}

export default Child


```
#### Difference btw Context Api vs Redux

| Context Api   | Redux |
| ------------- | ------------- |
| Inbuilt in React  | require external javascrpt library  |
| Require minimal setup  | Requires extensive setup to integrate it with a React Application  |
| It can be applied at a all application or a perticular no of components | It applied complete App |
| UI logic and State Management Logic are in the same component |  Better code organization with separate UI logic and State Management Logic |

## React useMemo Hook
> The React useMemo Hook returns a memoized value.
>  memoization as caching a value so that it does not need to be recalculated.
> The useMemo Hook only runs when one of its dependencies update.
> This can improve performance.
> ###### The useMemo and useCallback Hooks are similar. The main difference is that useMemo returns a memoized value and useCallback returns a memoized function.

<!-- eg1 -->
```
multiCnt function should call when click on update count button but its calling when click on both button. so we will use useMemo hook to call only when click on update Count button. and make as a function multiCntMemo that use as a callback of multicnt.

import React,{useState,useMemo} from "react";

const App=()=>{
  const [cnt,setCnt]=useState(0);
  const [item,setItem]=useState(0);

  const multiCntMemo=useMemo(function multiCnt(){
    console.log("mutliCnt");
    return cnt*5;
  },[cnt]);
  
  return(
    <div className="App">
      <h1>UseMemo Hook </h1>
      <h1>Count:{cnt} </h1>
      <h1>item:{item} </h1>
      {/* <h2>{multiCnt()}</h2> */}
      <h2>{multiCntMemo}</h2>
      <button onClick={()=>setCnt(cnt+1)}>Update Count</button>
      <button onClick={()=>setItem(item+1)}>Update Item</button>
    </div>
  )
}


export default App

```
## React useCallback Hook
> The React useCallback Hook returns a memoized callback function.

> memoization as caching a value so that it does not need to be recalculated.
> 
> This allows us to isolate resource intensive functions so that they will not automatically run on every render.
> 
> The useCallback Hook only runs when one of its dependencies update.
> This can improve performance.

> ###### Why Use:- One reason to use useCallback is to prevent a component from re-rendering unless its props have changed.

```
<!-- isme jab me count button(+) per bhi click kar rha hu to bhi child(todo) component render ho rha jabki usko nhi hona chahye woh change tab hona chhaye jab uske props change ho so that's why use useCallback hook -->
import { useState } from "react";
import Todos from "./Components/Todos";

const App = () => {
  const [count, setCount] = useState(0);
  const [todos, setTodos] = useState([]);

  const increment=()=>{
    setCount((c)=>c+1);
  };
  const addTodo=()=>{
    setTodos((t)=>[
      ...t,
      "New Todo"
    ])
  }
  return (
    <div>
      <Todos todos={todos} addTodo={addTodo}/>
      <hr />
      <div>
        Count:{count}
        <button onClick={increment}>+</button>
      </div>
    </div>
  )
}

export default App

<!-- Todo.jsx -->
const Todos = ({todos,addTodo}) => {
    console.log("child render");
  return (
    <div>
        <h2>My Todos</h2>
        {todos.map((todo,index)=>{
            return(
                <p key={index}>{todo}</p>
            )
        })}
        <button onClick={addTodo}>Add Todo</button>
    </div>
  )
}

export default Todos

```
>##### solution: To fix this, we can use the useCallback hook to prevent the function from being recreated (yha per todo baar baar ban jaa rha tha) unless necessary.

```
<!-- App.jsx -->
import { useState,useCallback } from "react";
import Todos from "./Components/Todos";

const App = () => {
  const [count, setCount] = useState(0);
  const [todos, setTodos] = useState([]);

  const increment=()=>{
    setCount((c)=>c+1);
  };

  //useCall back return the memoize function
  const addTodo= useCallback(()=>{
    setTodos((t)=>[
      ...t,
      "New Todo"
    ])
  },[todos])
  
  return (
    <div>
      <Todos todos={todos} addTodo={addTodo}/>
      <hr />
      <div>
        Count:{count}
        <button onClick={increment}>+</button>
      </div>
    </div>
  )
}

export default App

<!-- todos.jsx -->
import React from 'react'
import { memo } from 'react'; // This lines is imp

const Todos = ({todos,addTodo}) => {
    console.log("child render");
  return (
    <div>
        <h2>My Todos</h2>
        {todos.map((todo,index)=>{
            return(
                <p key={index}>{todo}</p>
            )
        })}
        <button onClick={addTodo}>Add Todo</button>
    </div>
  )
}

export default memo(Todos) // This lines is imp
//because we memose function

```

## React memo
> Using memo will cause React to skip rendering a component if its props have not changed.
> This can improve performance.

>
solution: To fix this, we can use memo.
Use memoto keep the Todos component from needlessly re-rendering.
Wrap the Todos component export in memo:
>

##### Problem
>In this example, the Todos component re-renders even when the todos have not changed.
```
<!-- App.js (no change: only iske child ya component me change) -->
import { useState } from "react";
import Todos from "./Components/Todos";



const App = () => {
  const [count, setCount] = useState(0);
  const [todos, setTodos] = useState(["todo 1", "todo 2","todo3"]);

  const increment = () => {
    setCount((c) => c + 1);
  };

  return (
    <>
      <Todos todos={todos} />
      <hr />
      <div>
        Count: {count}
        <button onClick={increment}>+</button>
      </div>
    </>
  );
};

export default App;


```
> When you click the increment button, the Todos component re-renders.
>If this component was complex, it could cause performance issues.
> #### Solution;
>To fix this, we can use memo.
>Use memoto keep the Todos component from needlessly re-rendering.
>Wrap the Todos component export in memo:
```
import React from 'react'
import { memo } from 'react';


const Todos = ({ todos }) => {
    console.log("child render");
    return (
      <>
        <h2>My Todos</h2>
        {todos.map((todo, index) => {
          return <p key={index}>{todo}</p>;
        })}
      </>
    );
  };
  
  export default memo(Todos);
```


