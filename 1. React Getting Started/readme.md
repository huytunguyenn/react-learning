
## React Basics

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

A: Well, we can't. 'Cause they are sibling component => lift the state up and put it in their parent component (which is App component)/

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

## Modern JavaScript

### ECMAScript and TC39


### Variables and Block Scopes

Variables created without a declaration keyword (`var`, `let`, or `const`) are always global, even if they are created inside a function.

### Arrow Functions


### Object Literals


### Destructuring and Rest/Spread


### Template Strings


### Classes


### Promises and Async/Await



