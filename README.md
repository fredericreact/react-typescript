# React typescript


```javascript
npx create-react-app react-ts --template typescript
```

```javascript
const Todos: React.FC<{items:string[]}> = (props) => {
    return (
    <ul>
    {props.items.map((item) => (
        <li key={item}>{item}</li>
    ))}
    </ul>
)}


export default Todos;

```

Here to build my Functional component, I need to use a generic provided by Typescript package or React : React.FC

It will define the type of this function as a functional component.

It also give the access to children props.

Then we plug/merge it with the props this Function component will have between the angle brackets <> and defined by us.

And React.FC is generic because it can have different props types.

Here we are using a generic instead of defining one like before.

Also, here, angle brackets are use to specify the props to be used by the generic instead of defining a generic as before

<br>
 
## Adding a Data Model


```javascript
class Todo {
    id: string;
    text: string;


    constructor(todoText: string) {
        this.text = todoText;
        this.id = new Date().toISOString();
    }
}


export default Todo;

```

```javascript
import Todos from './components/Todos';
import Todo from './models/todo';


function App() {


  const todos = [
    new Todo('Learn React'),
    new Todo('Learn Typescript')
  ]


  return (
    <div >
     <Todos items={todos}/>
    </div>
  );
}


export default App;

```

```javascript
import Todo from "../models/todo";


const Todos: React.FC<{items:Todo[]}> = (props) => {
    return (
    <ul>
    {props.items.map((item) => (
        <li key={item.id}>{item.text}</li>
    ))}
    </ul>
)}


export default Todos;

```

## Create a TodoItem component

```javascript
const TodoItem: React.FC<{text:string}> = (props) => {
    return <li>{props.text}</li>
}


export default TodoItem;

```

```javascript
import Todo from "../models/todo";
import TodoItem from "./TodoItem";


const Todos: React.FC<{items:Todo[]}> = (props) => {
    return (
    <ul>
    {props.items.map((item) => (
        <TodoItem key={item.id} text={item.text}/>
    ))}
    </ul>
)}


export default Todos;

```

## Form Submissions In TypeScript Projects

```javascript
const NewTodo = () => {


    const submitHandler = (event: React.FormEvent) => {
        event.preventDefault();
    }




    return (
    <form onSubmit={submitHandler}>
        <label htmlFor="text">Todo text</label>
        <input type='text' id='text'/>
        <button>Add Todo</button>
    </form>
    )
}


export default NewTodo;

```

## useRef

```javascript
import {useRef} from 'react';


const NewTodo = () => {
    const todoTextInputRef = useRef<HTMLInputElement>(null);


    const submitHandler = (event: React.FormEvent) => {
        event.preventDefault();


        const enteredText = todoTextInputRef.current!.value;


        if (enteredText.trim().length === 0 ) {
            return;
        }


       
    }




    return (
    <form onSubmit={submitHandler}>
        <label htmlFor="text">Todo text</label>
        <input type='text' id='text' ref={todoTextInputRef}/>
        <button>Add Todo</button>
    </form>
    )
}


export default NewTodo;

```

Here there is a ! after current to assert that this value will never be null, because the useRef connection will always be established.

If I am not sure if it could be null or undefined and if the useRef connection has been established or not, I should put ? instead of ! 

## Function props


```javascript
import {useRef} from 'react';


const NewTodo: React.FC<{onAddTodo: (text:string)=> void}> = (props) => {
    const todoTextInputRef = useRef<HTMLInputElement>(null);


    const submitHandler = (event: React.FormEvent) => {
        event.preventDefault();


        const enteredText = todoTextInputRef.current!.value;


        if (enteredText.trim().length === 0 ) {
            return;
        }


        props.onAddTodo(enteredText);
    }




    return (
    <form onSubmit={submitHandler}>
        <label htmlFor="text">Todo text</label>
        <input type='text' id='text' ref={todoTextInputRef}/>
        <button>Add Todo</button>
    </form>
    )
}


export default NewTodo;

```

Here void, means the function will return nothing

```javascript
import NewTodo from './components/NewTodo';
import Todos from './components/Todos';
import Todo from './models/todo';


function App() {


  const todos = [
    new Todo('Learn React'),
    new Todo('Learn Typescript')
  ]




  const addTodoHandler = (todoText: string) => {


  }


  return (
    <div >
     <NewTodo onAddTodo={addTodoHandler}/>
     <Todos items={todos}/>
    </div>
  );
}


export default App;



```

## Managing State & TypeScript

```javascript
import { useState } from 'react';
import NewTodo from './components/NewTodo';
import Todos from './components/Todos';
import Todo from './models/todo';


function App() {


const [todos, setTodos] = useState<Todo[]>([])




  const addTodoHandler = (todoText: string) => {
    const newTodo = new Todo(todoText);




    setTodos( (prevTodos) => {
      return prevTodos.concat(newTodo);
    });
  }


  return (
    <div >
     <NewTodo onAddTodo={addTodoHandler}/>
     <Todos items={todos}/>
    </div>
  );
}


export default App;

```

## Remove Todo

```javascript
import classes from './TodoItem.module.css'


const TodoItem: React.FC<{text:string; onRemoveTodo: ()=> void}> = (props) => {
    return <li className={classes.item} onClick={props.onRemoveTodo}>{props.text}</li>
}


export default TodoItem;

```

```javascript
import Todo from "../models/todo";
import TodoItem from "./TodoItem";
import classes from './Todos.module.css'


const Todos: React.FC<{items:Todo[]; onRemoveTodo: (id:string)=> void}> = (props) => {
    return (
    <ul className={classes.todos}>
    {props.items.map((item) => (
        <TodoItem key={item.id} text={item.text} onRemoveTodo={props.onRemoveTodo.bind(null,item.id)}/>
    ))}
    </ul>
)}


export default Todos;

```

```javascript
import { useState } from 'react';
import NewTodo from './components/NewTodo';
import Todos from './components/Todos';
import Todo from './models/todo';




function App() {


const [todos, setTodos] = useState<Todo[]>([])




  const addTodoHandler = (todoText: string) => {
    const newTodo = new Todo(todoText);




    setTodos( (prevTodos) => {
      return prevTodos.concat(newTodo);
    });
  }


  const removeTodoHandler = (todoId:string) => {
    setTodos((prevTodos) => {
      return prevTodos.filter(todo => todo.id !== todoId)
    })
  };


  return (
    <div >
     <NewTodo onAddTodo={addTodoHandler}/>
     <Todos items={todos} onRemoveTodo={removeTodoHandler}/>
    </div>
  );
}


export default App;



```

## Context API

### Provide

```javascript
import React, { useState } from 'react';
import Todo from '../models/todo';


type TodosContextObj = {
   
        items:Todo[];
        addTodo: (text:string) => void;
        removeTodo: (id: string) => void;
   
}


export const TodosContext = React.createContext<TodosContextObj>({
    items: [],
    addTodo: () => {},
    removeTodo: (id:string) => {}
})


const TodosContextProvider: React.FC = (props) => {


    const [todos, setTodos] = useState<Todo[]>([])




  const addTodoHandler = (todoText: string) => {
    const newTodo = new Todo(todoText);




    setTodos( (prevTodos) => {
      return prevTodos.concat(newTodo);
    });
  }


  const removeTodoHandler = (todoId:string) => {
    setTodos((prevTodos) => {
      return prevTodos.filter(todo => todo.id !== todoId)
    })
  };


  const contextValue: TodosContextObj = {
    items: todos,
    addTodo: addTodoHandler,
    removeTodo: removeTodoHandler
  }


    return <TodosContext.Provider value={contextValue}>
        {props.children}
    </TodosContext.Provider>
   
}


export default TodosContextProvider;

```

```javascript

import NewTodo from './components/NewTodo';
import Todos from './components/Todos';
import TodosContextProvider from './store/todos-context';


function App() {






  return (
    <TodosContextProvider>
     <NewTodo/>
     <Todos />
     </TodosContextProvider>
  );
}


export default App;

```

### use context API

```javascript
import TodoItem from "./TodoItem";
import classes from './Todos.module.css'
import React, {useContext} from 'react'
import { TodosContext } from "../store/todos-context";


const Todos: React.FC = () => {
    const todosCtx = useContext(TodosContext);


    return (
    <ul className={classes.todos}>
    {todosCtx.items.map((item) => (
        <TodoItem key={item.id} text={item.text} onRemoveTodo={todosCtx.removeTodo.bind(null,item.id)}/>
    ))}
    </ul>
)}


export default Todos;

```

```javascript
import {useRef, useContext} from 'react';
import classes from './NewTodo.module.css'
import { TodosContext } from '../store/todos-context';


const NewTodo: React.FC = () => {
    const todosCtx = useContext(TodosContext)


    const todoTextInputRef = useRef<HTMLInputElement>(null);


    const submitHandler = (event: React.FormEvent) => {
        event.preventDefault();


        const enteredText = todoTextInputRef.current!.value;


        if (enteredText.trim().length === 0 ) {
            return;
        }


        todosCtx.addTodo(enteredText)
    }




    return (
    <form onSubmit={submitHandler} className={classes.form}>
        <label htmlFor="text">Todo text</label>
        <input type='text' id='text' ref={todoTextInputRef}/>
        <button>Add Todo</button>
    </form>
    )
}


export default NewTodo;

```
