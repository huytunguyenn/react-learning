## Returns a function expression, JS closure

# Counter Dilemma Example

```javascript
// Initiate counter
let counter = 0;

// Function to increment counter
function add() {
  counter += 1;
}

// Call add() 3 times
add();
add();
add();

// The counter should now be 3
```
There is a problem with the solution above: Any code on the page can change the counter, without calling `add()`. The counter should be local to the add() function, to prevent other code from changing it 

=> A JavaScript **inner function** can solve this.

### Nested Functions

all functions have access to the scope "above" them.
```javascript
function add() {
  let counter = 0;
  function plus() {counter += 1;}
  plus();   
  return counter;
}
// the inner function plus() has access to the counter variable in the parent function:
```
This could have solved the counter dilemma, if we could reach the `plus() `function from the outside.

We also need to find a way to execute counter = 0 only once.

=> We need a **closure**.

### Closures

```javascript
const add = (function () {
  let counter = 0;
  return function () {counter += 1; return counter}   // The variable `add` is assigned to the return value of a self-invoking function.
})();

add();
add();
add();

// the counter is now 3
```
**Explained**:



The self-invoking function only runs once. It sets the counter to zero (0), and **returns a function expression**.

This way `add` becomes a function. The "wonderful" part is that it can access the counter in the _parent scope_.

This is called a **JavaScript closure**. It makes it possible for **a function to have "private" variables**.

The `counter` is protected by the scope of the anonymous function, and can only be changed using the `add` function.