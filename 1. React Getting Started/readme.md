
## React Basics

### TL;DR:

> - Components & reactive updates
> - Virtual DOM nodes and JSX
> - Props & State:
>  - (props) => {}
>  - [val, setVal] = useState(initialVal)
>  - Immutable props. Mutable state
> - ReactDOM.render
>  - arg1: \<Component/>
>  - arg2: DOM node 
> - React events: onClick, onSubmit, ...
> - Functions & class components

---

React component should start with uppercase, because React treats lowercase as HTML element.

```const a, setA = useState(x)``` results:
- a: state object (getter)
- setA: updater function (setter)
- x: initial value

Use curly braces `{expression}` to write Js code.

```javascript
onClick = {functionRef()}   // not work

onClick = {functionRef}     // just pass the pointer to the function, or
onClick = {() => {...}}      
```

`Return` in component:

```javascript
return {};      // not this, don't return object
return();       // this return function call (React.createElement())
```

Rendering many components, 3 ways to do that:
```javascript

ReactDOM.render(
    <Button/> <Display/>,         // not gonna work cause each will return as function call
    document.getElementById('mountNode')
);

ReactDOM.render(
    [<Button/>, <Display/>],      // array, this should works  
    document.getElementById('mountNode')
);

ReactDOM.render(
    // <div>                         // React API supports this
    //     <Button/>                
    //     <Display/>
    // </div>    
    <React.Fragment>                // but u should use this, or shortcut <>...</>
        <Button/>   
        <Display/>    
    </React.Fragment>        
    document.getElementById('mountNode')
);
```
but you should combine children into an element like this:
```javascript
function App(){
    return(
        <div>
            <Button/>
            <Display/>
        </div>    
    );
}

ReactDOM.render(
    <App/>,
    document.getElementById('mountNode')
);

```
**Q: When we have state element in Button component, and we want access that in Display component. How?** 

A: Well, we can't. 'Cause they are sibling component => lift the state up and put it in their parent component (which is App component)

=> One-way flow of data. Parent components can flow their data down to children components, they can even flow behaviors down too (pass function).

```javascript
// this not gonna work, cause we need a function reference for the onClickEvent handler
<button onClick = {props.onClickFunction(props.increment)}> 
    add {props.increment} 
</button>

// cái phía trên ko phải reference mà là invocation (invoke a function, call a function, trong JS code & variable đc thực thi lúc gọi hàm-invoke (chứ ko phải lúc đc define), đc hủy lúc thực thi xong hàm)
// so we gonna wrap invocation (gọi hàm) in an inline function to make it into a reference
<button onClick = {() => props.onClickFunction(props.increment)}>
    add {props.increment}
</button>


// cleaner
const handleClick = () => props.onClickFunction(props.increment)

<button onClick = {handleClick}>
    add {props.increment}
</button>
```
Read more: [JS closure & return function expression](../js-closure.md)

###Magic of React Virtual DOM

```javascript
const render = () => {
    // 1. with HTML
    document.getElementById('mountNode').innerHTML = `
    <div>
        Hello HTML
        <input/>
        <pre> ${(new Date).toLocaleDateString()}  </pre>
    </div>
    `;
    
    // 2. with React
    ReactDOM.render(
        React.createElement(
            'div',
            null,
            'Hello React',
            React.createElement('input', null),
            React.constructor('pre', null, (new Date).toLocaleDateString())
        ),
        document.getElementById('mountNode2'),
    );
}

setInterval(render, 1000);      // mỗi giây gọi hàm render để render
```

Đoạn code trên hiển thị 1 input và 1 bộ đếm thời gian. Với code bằng HTML, ta ko thể giữ lại những gì gõ vào input vì mỗi giây toàn bộ HTML đều bị render lại => những gì trong input đều bị mất luôn

Nhưng với React, Virtual DOM chỉ render lại thẻ `<pre/>`, nên ta có thể gõ vào input mà ko sợ bị reset lại mỗi giây

---

## Modern JavaScript (ECMAScript and TC39)

### Scope

```javascript
function sum(a, b){
    // function scope
    var result = a + b;
}
result      // not defined
```
The problem with `var` in block scope,
```javascript
for (var i = 1; i <= 10; i++){
    // block scope
}
i   // it's 11, weird!

for (let i = 1; i <= 10; i++){
    // block scope
}
i   // not defined
```
and in nested scope:
```javascript
{
    // Block scope
    {
        // Nested block scope
        // using let & const will protect variable in this scope
    }
}
```
### Variables

Variables created without a declaration keyword (`var`, `let`, or `const`) are always global, even if they are created inside a function.

```javascript
// Scalar values 
// => can't be changed
const answer = 42;
const greeting = 'Hello';

// Arrays and Objects 
// => this just the pointer to the object (the object's content itself can be changed)
const numbers = [2, 4, 6];
const person = {
    firstName: 'John',
    lastName: 'Doe',
};
```
Most of the case, you should use `const`, then `let`. Never use `var`.

### Arrow Functions

```javascript
const X = function () {
    // `this` here is the caller of X (vd có thẻ button gọi X thì this là thẻ button đó)
}

const Y = () => {
    // `this` here is Y
}
```

### Object Literals

```javascript
const mystery = 'answer';
const InverseOfPI = 1 / Math.PI;

const obj = {
    p1: 10, 
    p2: 20, 
    f1() {}, 
    f2: () => {}, 
    [mystery]: 42,      // dynamic property, not array lol
    InverseOfPI,        // instead of `InverseOfPI: InverseOfPI`
};

console.log(obj.mystery)        // undefined
console.log(obj.answer)        // 42
```

### Destructuring 
Destructuring object:
```javascript
// instead of
const PI = Math.PI;
const E = Math.E;
const SQRT2 = Math.SQRT2;
// use this
const {PI, E, SQRT2}  = Math;

// Somewhere in a React App
const {Component, Fragment, useState} = require('react');
useState();
```

```javascript
const circle = {
  label: 'circleX',
  radius: 2,
};

// {radius}: kéo radius ra từ object circle đc truyền vào
// {precision = 2} = {}: nghĩa là làm cho object này là optional, default value cho precision là 2
const circleArea = ({radius}, {precision = 2} = {}) =>
  (PI * radius * radius).toFixed(precision);

console.log(
  circleArea(circle, { precision: 5 })
);
```
Destructuring array:
```javascript
const [first, second,, forth] = [10, 20, 30, 40];
const [value, setValue] = useState(initialValue);
```
### Rest/Spread
Rest: gộp chung vô thành 1 cái

Spread: tách ra thành nhiều cái

With array:
```javascript
const [first, ...restOfItems] = [10, 20, 30, 40];   // combine array destructuring with REST operator

first;          // 10    
restOfItems;    // [20, 30, 40]

// SPREAD the items in an array into a new array
const newArray = [...restOfItems];
```
With object:
```javascript
const data = {
    temp1: '001', 
    temp2: '002', 
    firstName: 'John', 
    lastName: 'Doe',
};

// destructure temp1 & temp1 and use REST operator to get the remaining props into a new object - person
const {temp1, temp2, ...person} = data;

// SPREAD the items into a new object
const newObject = {
    ...person
}
```
### Template Strings
Dynamic content in string
```javascript
const html = `
    <div> 
        ${Math.random()} 
    </div>
`;       
// <div>
//      0.234453
// </div>
```

### Classes

```javascript
class Person {
  constructor(name) {
    this.name = name;
  }
  greet() {
    console.log(`Hello ${this.name}!`);
  }
}

class Student extends Person {
  constructor(name, level) {
    super(name);
    this.level = level;
  }
  greet() {
    console.log(`Hello ${this.name} from ${this.level}`);
  }
}

const o1 = new Person("Max");
const o2 = new Student("Tina", "1st Grade");
const o3 = new Student("Mary", "2nd Grade");
o3.greet = () => console.log('I am special!');

o1.greet();
o2.greet();
o3.greet();
```

### Promises and Async/Await

```javascript
// fetch return a promise, then use `then` and pass a callback function
// json() is also asynchronous, so it return a promise
const fetchData = () => {
  fetch('https://api.github.com').then(resp => {
    resp.json().then(data => {
      console.log(data);
    });
  });
};

// the above syntax is complicated
// => use async function & await promise
const fetchData = async () => {
  const resp = await fetch('https://api.github.com');
  const data = await resp.json();
  console.log(data);
};

fetchData(); // asynchronous, return a promise object

```

