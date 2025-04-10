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
  },
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
const b = { key: "b" };
const c = { key: "c" };

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
  console.log("Promise created");
  resolve("Done");
});

promise.then((res) => console.log(res));

console.log("After promise");
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
console.log("Start");

setTimeout(() => {
  console.log("Timeout");
}, 0);

Promise.resolve().then(() => {
  console.log("Promise");
});

console.log("End");
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
console.log([] == false); // true
console.log([] == ![]); // true
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
  return "💥 Fireball!";
};

const merlin = new Wizard();
merlin.castSpell = () => "✨ Sparkles!";
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
console.log(arr.map((x) => x * 2)); // ??
```

<details>
<summary>💡 Answer</summary>

- `arr.length` is `11` (last index + 1).
- `map` skips empty slots, so `[2, 4, 6, <7 empty items>, 22]`.

</details>

---

## 🧪 9. Falsy Funhouse

```js
const weird = [0, null, undefined, false, "", NaN];

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
