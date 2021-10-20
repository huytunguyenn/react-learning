# Returns a function expression, JS closure

## Counter Dilemma Example

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

## Nested Functions

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

## Closures

```javascript
const add = (function () {
  let counter = 0;
  return function () {counter += 1; return counter}   
})();

add();
add();
add();

// the counter is now 3
```
**Explained**:

- The variable `add` is assigned to the return value of a self-invoking function.
 
- The self-invoking function only runs once. It sets the counter to zero (0), and **returns a function expression**.

- This way `add` becomes a function. The "wonderful" part is that it can access the counter in the _parent scope_.

- This is called a **JavaScript closure**. It makes it possible for **a function to have "private" variables**.

- The `counter` is protected by the scope of the anonymous function, and can only be changed using the `add` function.

Closure một chức năng có quyền truy cập vào phạm vi cha, ngay cả sau khi phạm vi đã đóng 

(ở vd này hảm tăng counter vẫn có quyền truy cập biến counter của hàm cha của nó, mặc dùng hàm cha chỉ đc chạy 1 lần duy nhất (khởi tạo counter = 0))

**Ví dụ khác:**

```javascript
function name(n) {
  return function(a) {
    return `${n} likes ${a}`;
  };
}

var j = name('Tú');

j;
//  function (a) {
//    return `${n} likes ${a}`;
//  }

j('Ngọc');    // 'Tú likes Ngọc'
j('Sana');    // 'Tú likes Sana'
```

## Áp dụng để tạo biến private trong class

Closure giúp xác định các biến private (ở đây `setName` và `getName` đc bind với `this` nên có thể truy cập bên ngoài được, trong khi `_name` chỉ là 1 biến được định nghĩa bên trong hàm cha, bên ngoài ko truy cập đc, nhưng 2 hàm con bên trong thì có thể)

```javascript
function Product() {
    let _name;              // thường quy ước _name là biến private

    this.setName = function(value) {
        _name = value;
    };

    this.getName = function() {
        return _name;
    };
}

let p = new Product();
p.setName("anonystick.com");

console.log(p._name);             // undefined
console.log(p.getName());         // anonystick.com
```