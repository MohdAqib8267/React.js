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



