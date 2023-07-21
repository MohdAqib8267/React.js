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
