# 🧩 JavaScript Bug Riddles

A collection of fun and tricky JavaScript bugs that test your understanding of how JavaScript really works.  
Great for workshops, interviews, or self-study!

---

## 🔁 1. Loop Closure Madness

```js
const buttons = [];

for (var i = 0; i < 3; i++) {
  buttons.push(function () {
    console.log(`Button ${i} clicked`);
  });
}

buttons[0](); // ??
buttons[1](); // ??
buttons[2](); // ??
```

<details>
<summary>💡 Answer</summary>

They all log `Button 3 clicked`.

Because `var` is function-scoped, not block-scoped, all closures share the same `i` — which ends at 3 after the loop.

</details>

---

## 🧠 2. The Curious Case of `this`

```js
const obj = {
  value: 42,
  getValue: () => {
    return this.value;
  }
};

console.log(obj.getValue()); // ??
```

<details>
<summary>💡 Answer</summary>

Logs `undefined`.

Arrow functions don't bind their own `this` — they use the one from their outer lexical scope (likely the global object in this case).

</details>

---

## 📦 3. Object Keys That Disappear

```js
const a = {};
const b = { key: 'b' };
const c = { key: 'c' };

a[b] = 123;
a[c] = 456;

console.log(a[b]); // ??
```

<details>
<summary>💡 Answer</summary>

Logs `456`.

Objects used as keys are converted to the same string: `"[object Object]"`. So `b` and `c` overwrite the same key.

</details>

---

## 💤 4. The Lazy Promise

```js
const promise = new Promise((resolve, reject) => {
  console.log('Promise created');
  resolve('Done');
});

promise.then(res => console.log(res));

console.log('After promise');
```

<details>
<summary>💡 Answer</summary>

Logs:

```
Promise created
After promise
Done
```

Creating a Promise runs its executor function immediately (sync). `.then()` runs async (microtask).

</details>

---

## 🧵 5. Microtasks vs Macrotasks

```js
console.log('Start');

setTimeout(() => {
  console.log('Timeout');
}, 0);

Promise.resolve().then(() => {
  console.log('Promise');
});

console.log('End');
```

<details>
<summary>💡 Answer</summary>

Logs:

```
Start
End
Promise
Timeout
```

Microtasks (`Promise.then`) run before macrotasks (`setTimeout`), even with 0 delay.

</details>

---

## 🔄 6. Reverse Coercion Riddle

```js
console.log([] == false);     // true
console.log([] == ![]);       // true
```

<details>
<summary>💡 Answer</summary>

- `[] == false` → `true` due to coercion: `[]` → `''` → `false`.
- `[] == ![]` → `[] == false` → again `true`.

</details>

---

## 🧙‍♂️ 7. The Case of the Invisible Method

```js
function Wizard() {}
Wizard.prototype.castSpell = function () {
  return '💥 Fireball!';
};

const merlin = new Wizard();
merlin.castSpell = () => '✨ Sparkles!';
delete merlin.castSpell;

console.log(merlin.castSpell()); // ??
```

<details>
<summary>💡 Answer</summary>

Logs `'💥 Fireball!'`.

Deleting the instance method reveals the prototype method.

</details>

---

## 🧭 8. Array Gaps

```js
const arr = [1, 2, 3];
arr[10] = 11;

console.log(arr.length); // ??
console.log(arr.map(x => x * 2)); // ??
```

<details>
<summary>💡 Answer</summary>

- `arr.length` is `11` (last index + 1).
- `map` skips empty slots, so `[2, 4, 6, <7 empty items>, 22]`.

</details>

---

## 🧪 9. Falsy Funhouse

```js
const weird = [0, null, undefined, false, '', NaN];

for (let value of weird) {
  if (value) {
    console.log(`${value} is truthy`);
  } else {
    console.log(`${value} is falsy`);
  }
}
```

<details>
<summary>💡 Answer</summary>

All log as falsy. These are the six falsy primitives in JS.

</details>

---

## 🕳️ 10. Hoisting Hole

```js
function test() {
  console.log(value);
  let value = 10;
}

test();
```

<details>
<summary>💡 Answer</summary>

Throws a `ReferenceError`. `let` is hoisted but not initialized — it's in the Temporal Dead Zone.

</details>


---

## 🧩 11. typeof null

```js
console.log(typeof null); // ??
```

<details>
<summary>💡 Answer</summary>

Returns `'object'`.

This is a long-standing bug in JavaScript — `null` is not actually an object, but `typeof null` returns `'object'`.

</details>

---

## 🧩 12. NaN is not NaN?

```js
console.log(NaN === NaN); // ??
```

<details>
<summary>💡 Answer</summary>

Returns `false`.

NaN is the only value in JS that is not equal to itself.

</details>

---

## 🧩 13. Chained Comparisons Lie

```js
console.log(3 > 2 > 1); // ??
```

<details>
<summary>💡 Answer</summary>

Returns `false`.

`3 > 2` → `true` → `true > 1` → `1 > 1` → `false`.

</details>

---

## 🧩 14. Function Declaration vs Expression

```js
hoisted();

function hoisted() {
  console.log('I am hoisted');
}

notHoisted();

var notHoisted = function () {
  console.log('I am not hoisted');
};
```

<details>
<summary>💡 Answer</summary>

- `hoisted()` runs fine.
- `notHoisted()` throws `TypeError: notHoisted is not a function`.

Only function declarations are hoisted with their definitions. `var` is hoisted as `undefined`.

</details>

---

## 🧩 15. Unexpected Implicit Globals

```js
function surprise() {
  oops = 42;
}
surprise();
console.log(oops); // ??
```

<details>
<summary>💡 Answer</summary>

Logs `42`.

Undeclared variables become *implicit globals* (in sloppy mode).

</details>

---

## 🧩 16. Destructuring Undefined

```js
const { length } = undefined;
```

<details>
<summary>💡 Answer</summary>

Throws `TypeError: Cannot destructure property`.

You can't destructure properties from `undefined` or `null`.

</details>

---

## 🧩 17. Array Holes Aren’t Undefined

```js
const arr = [1, , 3];

console.log(arr[1]); // ??
console.log(1 in arr); // ??
```

<details>
<summary>💡 Answer</summary>

- `arr[1]` logs `undefined`.
- `1 in arr` returns `false`.

There's a *hole* in the array, not an actual `undefined` value.

</details>

---

## 🧩 18. setTimeout in Loops

```js
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 0);
}
```

<details>
<summary>💡 Answer</summary>

Logs: `3`, `3`, `3`.

All callbacks share the same `i` (3) due to `var`. Use `let` to capture each iteration.

</details>

---

## 🧩 19. The Missing Return

```js
function foo() {
  return
    {
      ok: true
    };
}

console.log(foo()); // ??
```

<details>
<summary>💡 Answer</summary>

Returns `undefined`.

The `return` statement is terminated before the object — it's a line break bug.

</details>

---

## 🧩 20. parseInt Quirk

```js
console.log(parseInt('08')); // ??
```

<details>
<summary>💡 Answer</summary>

Returns `8`, but older engines may interpret `'08'` as octal and return `0`.

Always pass radix: `parseInt('08', 10)`.

</details>

---

## 🧩 21. isNaN vs Number.isNaN

```js
console.log(isNaN('foo'));        // ??
console.log(Number.isNaN('foo')); // ??
```

<details>
<summary>💡 Answer</summary>

- `isNaN('foo')` → `true` (due to coercion).
- `Number.isNaN('foo')` → `false`.

Use `Number.isNaN` to avoid coercion.

</details>

---

## 🧩 22. Arguments Object Trap

```js
function test(x) {
  arguments[0] = 99;
  return x;
}

console.log(test(42)); // ??
```

<details>
<summary>💡 Answer</summary>

Returns `99`.

In non-strict mode, `arguments` and named params are linked.

</details>

---

## 🧩 23. Double Negation Oddity

```js
console.log(!!'false' == !!'true'); // ??
```

<details>
<summary>💡 Answer</summary>

Returns `true`.

Both `'false'` and `'true'` are non-empty strings → truthy.

</details>

---

## 🧩 24. Math with Arrays

```js
console.log([1, 2] + [3, 4]); // ??
```

<details>
<summary>💡 Answer</summary>

Returns `'1,23,4'`.

Arrays are coerced to strings: `'1,2' + '3,4'`.

</details>

---

## 🧩 25. Number + Object

```js
console.log(1 + {}); // ??
```

<details>
<summary>💡 Answer</summary>

Returns `'1[object Object]'`.

Object gets coerced to string.

</details>

---

## 🧩 26. Object Coercion to Number

```js
console.log(+{}); // ??
```

<details>
<summary>💡 Answer</summary>

Returns `NaN`.

`+{}` → `Number('[object Object]')` → `NaN`.

</details>

---

## 🧩 27. String Padding Confusion

```js
console.log('5' - '2'); // ??
console.log('5' + '2'); // ??
```

<details>
<summary>💡 Answer</summary>

- `'5' - '2'` → `3` (coerced to numbers).
- `'5' + '2'` → `'52'` (string concatenation).

</details>

---

## 🧩 28. typeof Function.prototype

```js
console.log(typeof Function.prototype); // ??
```

<details>
<summary>💡 Answer</summary>

Returns `'function'`.

Function prototype is still a function.

</details>

---

## 🧩 29. Weird instanceof

```js
console.log([] instanceof Array);       // true
console.log([] instanceof Object);      // true
console.log(function(){} instanceof Function); // true
```

<details>
<summary>💡 Answer</summary>

They're all `true`.

Everything in JS is ultimately an object, and functions are also objects.

</details>

---

## 🧩 30. Deleting Variables

```js
var x = 10;
console.log(delete x); // ??
```

<details>
<summary>💡 Answer</summary>

Returns `false`.

`delete` only works on object properties, not variables declared with `var`, `let`, or `const`.

</details>
