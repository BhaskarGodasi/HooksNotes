# React Hooks

Hooks in React are built-in functions that allow developers to use lifecycle and state functions inside functional components.

## Table of Contents

1.  [useState](#orge14fed7)
2.  [useEffect](#org24ad32c)
3.  [useRef](#org9105d5d)
4.  [useCallback](#org847ed77)
5.  [useMemo](#org08466be)
6.  [useContext](#org6284874)
7.  [UseReducer](#org0c74463)

With Hooks, developers get to enjoy powerful functional components for almost everything. From rendering UI to handling state logic Hooks can do everything.

<a id="orge14fed7"></a>

## useState()

The useState hook allows us to create state variables in a React function component.

```
State allows us to access and update certain values in our components over time
```

### Syntax

suppose we have a state with name data, let's see how we can define that by using &rsquo;useState hooks&rsquo;

` const [data , setData] = useState(' ')`


![image](https://media.graphassets.com/AyJLadrgSFGtKmaeZuvU  "a state")


In initial state we can define any data type like

<ol>
<li>Number
<li>String
<li>Boolean
<li>Null
<li>Object
<li>Array</li>
and even
<li>Function()

let see how to update the state by using following example

    function Greeting() {
      const [name, setName] = useState("Prakash")

    const handleChange=(e)=>{
        setName(e.traget.value)
    }

      return (
          <div>
            <form>

              <input type="text" onChange={handleChange} id="name" />

            </form>
            {name }
          </div>
      )
    }

<a id="org24ad32c"></a>

## useEffect()

The main functionality is that the useEffect will happen upon the initial render of that component, and allows for changes to re-render that component (items to be watched for change must be included in the dependency array, which we will touch on later).

useEffect lets us perform side effects in function components.

```
 Side effects are when we need to reach into the outside world. Such as fetching data from an API or working with the DOM.
```


**The core concepts of useEffect**
<ol>
<li>Fetching data
<li>Reading from local storage
<li>Registering and deregistering event listeners
</ol>



### Syntax

      useEffect(() => {
         *fetch data, set event listeners, etc*
      }, [optional dependency array])

**_Mistake made by new learner's_**

If you do not provide the dependencies array at all and only provide a function to useEffect, it will run after every render.

       function MyComponent() {
        const [data, setData] = useState([])

           useEffect(() => {
           fetchData().then(myData => setData(myData))
           // Error! useEffect runs after every render without the dependencies array, causing infinite loop
           });
        }

**Example:-**

**_Correct way to pass useEffect_**

    import { useEffect } from 'react';

     function MyComponent() {
        const [data, setData] = useState([])

        useEffect(() => {
           fetchData().then(myData => setData(myData))
           // Correct! Runs once after render with empty array
         }, []);

       return <ul>{data.map(item => <li key={item}>{item}</li>)}</ul>
    }



***The key concepts of using effects***

we should summarize the main concepts you’ll need to understand to master useEffect

<ul>
<li>You must thoroughly understand when components (re-)render because effects run after every render cycle
<li>Effects are always executed after rendering, but you can opt-out of this behavior
<li> An effect is only rerun if at least one of the values specified as part of the effect’s dependencies has changed since the last render cycle
<li>You should ensure that components are not re-rendered unnecessarily. This constitutes another strategy to skip unnecessary reruns of effects.
<li>You have to understand that functions defined in the body of your function component get recreated on every render cycle. This has an impact if you use it inside of your effect.

<a id="org9105d5d"></a>

## useRef()

Refs are a special attribute that are available on all React components. They allow us to create a reference to a given element / component when the component mounts.

useRef takes in a single function argument, initialValue , and returns a mutable reference object whose .current value is initialized to the passed argument. useRef is essentially a “box” whose current key holds a mutable value.

     const refContainer = useRef(initialValue);

useRef serves a couple main purposes:

 <ol>
 <li> Access DOM Elements.<li>
 <li> Persist values across successive renders<li>
 </ol>

### Syntax

Accessing DOM Elements instead of traditional document.getElementById or any other document method .
we are going use &rsquo;useRef hooks&rsquo;

**_ Example _**

    import React from "react";

        const Form = (props) => {
            const inputRef = React.useRef(null);
             React.useEffect(() => {
             console.log(inputRef.current);
               inputRef.current.focus();
               }, []);

         return (
           <form>
             <input
             type="text"
             placeholder="Enter Name"
             name="name"
             ref={inputRef} />
           </form>
       );

      };

    export default Form;


***Persist Values Across Successive***

There are situations where you may want to persist values across renders instead of recreating them on each render. Instead of storing these values in states which may cause additional unnecessary rendering, you can put them in useRef ‘s current property. 


       const Timer = () => {
             const intervalRef = useRef();

             useEffect(() => {
                 const id = setInterval(() => {
                 // ...
                });
                intervalRef.current = id;
                return () => {
                  clearInterval(intervalRef.current);
                 };
             });

            // ...
       }

<a id="org847ed77"></a>

## useCallback()


 when a component re-renders, every function inside of the component is recreated and therefore these functions’ references change between renders.


```
Callback functions are the name of functions that are "called back" within a parent component.
```

 useCallback(callback, dependencies) will return a memoized instance of the callback that only changes if one of the dependencies has changed.

  This means that instead of recreating the function object on every re-render, we can use the same function object between renders.

### Syntax


     export default function ParentComponent() {
        const [state, setState] = useState(false);
        const [dep] = useState(false);
        console.log("Parent Component redered");

        const handler = useCallback(
             (event) => {
              console.log("You clicked ", event.currentTarget);
             },[dep],
         );
         const statehanddler = () => {
          setState(!state);
         };
        return (
         <>
            <button onClick={statehanddler}>Change State Of Parent Component</button>
             <MyList handler={handler} />
         </>
     );



useCallback() has its performance drawbacks, as it still has to run on every component re-render.

In below example, useCallback() is not helping optimization, since we’re creating the clickHandler function on every render anyways;

         export default function MyComponent() {
    
         // poor usage of useCallback()
           const clickHandler = useCallback(() => {
              // handle the click event
           }, []);

         return <ButtonWrapper onClick={clickHandler} />;
        }

         const ButtonWrapper = ({ clickHandler }) => {
             return <button onClick={clickHandler}>Child Component</button>;
         };

<a id="org08466be"></a>

## useMemo()
<ul>
<li>useMemo enables memoization in React hook functions. Memoization is a programming strategy where the function remembers the output of previous executions and uses it for further executions.


<li>If the memoized function is called again with the same parameters, it doesn’t re-execute the function. Rather, the initial cached value is returned from the function, reducing the overhead of executing the same function again.

<li>React’s useMemo hook enables us to memoize the result of the execution of a function with a given set of parameters. Next time the function is called with the same parameter, we can return the data that has been cached, rather than re-executing the entire function.


    import React, { useState, useMemo } from "react";

     export default function SimpleStateHooks() { 
         const [count, setCount] = useState(1000);

          function returnValue(inputValue) {
              return inputValue + 10;
           }

        var calculatedValue = useMemo(() => returnValue(10), [10]);

          setTimeout(() => {
              setCount(count + 1);
            }, 1000)

         return (
         <div>
            <p>You clicked {count} times</p>

            <p>Random Value: {calculatedValue}</p>

             <button onClick={() => setCount(count + 1)}>Update Count</button>
         </div>
       );
     }
<a id="org6284874"></a>

## useContext()


***Context***

The React Context API was introduced to overcome the problem of passing props down the tree of components. The state of the application can be stored globally, and shared across multiple components. Now with the addition of hooks to the React architecture, we get a new hook called useContext.

***useContext***

The useContext hook, just like all the other hooks we have seen before, is easier to write and works with React’s functional components. Previously, we could use the Context API only within class components. But with this hook, we can literally use context, within functional components with ease. It is also much easier to read and write.

```
In some cases, it is fine to pass props through multiple components, but it is redundant to pass props through components which do not need it.
```


The useContext hook removes the unusual-looking render props pattern that was required in consuming React Context before.

### Syntax 
*** Sample Data component ***


         const blogInfo = {
             React: {
             post: "Learn useContext Hooks",
             author: "Adhithi Ravichandran"
              },
             GraphQL: {
             post: "Learn GraphQL Mutations",
             author: "Adhithi Ravichandran"
             }
        };


*** Parent Component ***


      function ParentComponent() {
          const blogInfoContext = React.createContext(blogInfo);
      return (
         <blogInfoContext.Provider value={blogInfo}>
         <h2>Use Context Example Component</h2>
         <ChildComponent />
         
       </blogInfoContext.Provider>
      );
    }


*** Child Component ***


      export function ChildComponent() {
       const blogDetails = useContext(blogInfoContext);
       return (
            <div>
            <h3>React Blog Details</h3>
            <p>Topic: {blogDetails.React.post}</p>
            <p>Author: {blogDetails.React.author}</p>
            </div>
          );
      }


In this we can pass data through nth child of component in project folder.

<a id="org0c74463"></a>

## useReducer()

useReducer() is an alternative to useState(). It accepts a reducer function and a second argument initial state, it returns the current state paired with a dispatch function.

`const [state, dispatch] = useReducer(reducer, initialState);`

![image](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRDd74htkw3SG5jn4w2uOTu_a-H_IKqqFq4PnVGkldpCA&usqp=CAU&ec=48665701 'useReducer')


```
Reducers are simple, predictable (pure) functions that take a previous state object and an action object and return a new state object.
```

   
     import React, { useReducer } from "react";
     // initial component state
     const initialState = { count: 0 };
     
      const ACTIONS = {
        DECREMENT: "decrement",
        INCREMENT: "increment",
       };

     // (currentState,actionObject)=> newState
     function reducer(state, action) {
      switch (action.type) {
          case "increment":
             return { count: state.count + 1 };
          case "decrement":
             return { count: state.count - 1 };
          default:
             throw new Error();
        }
    }

    export default function Counter() {
       const [state, dispatch] = useReducer(reducer, initialState);
       return (
       <>
          <button onClick={() => {
            // dispatch dispatches the action object to the reducer function 
          dispatch({ type: ACTIONS.DECREMENT })}}>
           decrement
           </button>
           <span>Count: {state.count}</span>
           <button onClick={() => dispatch({ type: ACTIONS.INCREMENT })}>
           increment
           </button>
       </>
        );
      }



I hope this material will helpfull.  😊
