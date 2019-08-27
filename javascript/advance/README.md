###### 1. What's the output?

```javascript
const shape = {
  radius: 10,
  diameter() {
    return this.radius * 2;
  },
  perimeter: () => 2 * Math.PI * this.radius
};

console.log(shape.diameter());
console.log(shape.perimeter());
```

- A: `20` and `62.83185307179586`
- B: `20` and `NaN`
- C: `20` and `63`
- D: `NaN` and `63`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: B

Note that the value of `diameter` is a regular function, whereas the value of `perimeter` is an arrow function.

With arrow functions, the `this` keyword refers to its current surrounding scope, unlike regular functions! This means that when we call `perimeter`, it doesn't refer to the shape object, but to its surrounding scope (window for example).

There is no value `radius` on that object, which returns `undefined`.

</p>
</details>

---

###### 2. What's the output?

```javascript
class Chameleon {
  static colorChange(newColor) {
    this.newColor = newColor;
    return this.newColor;
  }

  constructor({ newColor = "green" } = {}) {
    this.newColor = newColor;
  }
}

const freddie = new Chameleon({ newColor: "purple" });
console.log(freddie.colorChange("orange"));
```

- A: `orange`
- B: `purple`
- C: `green`
- D: `TypeError`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: D

The `colorChange` function is static. Static methods are designed to live only on the constructor in which they are created, and cannot be passed down to any children. Since `freddie` is a child, the function is not passed down, and not available on the `freddie` instance: a `TypeError` is thrown.

</p>
</details>

---

###### 3. What's the output?

```javascript
function Person(firstName, lastName) {
  this.firstName = firstName;
  this.lastName = lastName;
}

const member = new Person("Lydia", "Hallie");
Person.getFullName = function() {
  return `${this.firstName} ${this.lastName}`;
};

console.log(member.getFullName());
```

- A: `TypeError`
- B: `SyntaxError`
- C: `Lydia Hallie`
- D: `undefined` `undefined`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A

You can't add properties to a constructor like you can with regular objects. If you want to add a feature to all objects at once, you have to use the prototype instead. So in this case,

```js
Person.prototype.getFullName = function() {
  return `${this.firstName} ${this.lastName}`;
};
```

would have made `member.getFullName()` work. Why is this beneficial? Say that we added this method to the constructor itself. Maybe not every `Person` instance needed this method. This would waste a lot of memory space, since they would still have that property, which takes of memory space for each instance. Instead, if we only add it to the prototype, we just have it at one spot in memory, yet they all have access to it!

</p>
</details>

---

###### 4. What's the output?

```javascript
const obj = { 1: "a", 2: "b", 3: "c" };
const set = new Set([1, 2, 3, 4, 5]);

obj.hasOwnProperty("1");
obj.hasOwnProperty(1);
set.has("1");
set.has(1);
```

- A: `false` `true` `false` `true`
- B: `false` `true` `true` `true`
- C: `true` `true` `false` `true`
- D: `true` `true` `true` `true`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: C

All object keys (excluding Symbols) are strings under the hood, even if you don't type it yourself as a string. This is why `obj.hasOwnProperty('1')` also returns true.

It doesn't work that way for a set. There is no `'1'` in our set: `set.has('1')` returns `false`. It has the numeric type `1`, `set.has(1)` returns `true`.

</p>
</details>

---

###### 5. What's the output?

```javascript
(() => {
  let x, y;
  try {
    throw new Error();
  } catch (x) {
    (x = 1), (y = 2);
    console.log(x);
  }
  console.log(x);
  console.log(y);
})();
```

- A: `1` `undefined` `2`
- B: `undefined` `undefined` `undefined`
- C: `1` `1` `2`
- D: `1` `undefined` `undefined`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A

The `catch` block receives the argument `x`. This is not the same `x` as the variable when we pass arguments. This variable `x` is block-scoped.

Later, we set this block-scoped variable equal to `1`, and set the value of the variable `y`. Now, we log the block-scoped variable `x`, which is equal to `1`.

Outside of the `catch` block, `x` is still `undefined`, and `y` is `2`. When we want to `console.log(x)` outside of the `catch` block, it returns `undefined`, and `y` returns `2`.

</p>
</details>

---

###### 6. What's the output?

```javascript
[[0, 1], [2, 3]].reduce(
  (acc, cur) => {
    return acc.concat(cur);
  },
  [1, 2]
);
```

- A: `[0, 1, 2, 3, 1, 2]`
- B: `[6, 1, 2]`
- C: `[1, 2, 0, 1, 2, 3]`
- D: `[1, 2, 6]`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: C

`[1, 2]` is our initial value. This is the value we start with, and the value of the very first `acc`. During the first round, `acc` is `[1,2]`, and `cur` is `[0, 1]`. We concatenate them, which results in `[1, 2, 0, 1]`.

Then, `[1, 2, 0, 1]` is `acc` and `[2, 3]` is `cur`. We concatenate them, and get `[1, 2, 0, 1, 2, 3]`

</p>
</details>

---

###### 7. What's the output?

```javascript
function* generator(i) {
  yield i;
  yield i * 2;
}

const gen = generator(10);

console.log(gen.next().value);
console.log(gen.next().value);
```

- A: `[0, 10], [10, 20]`
- B: `20, 20`
- C: `10, 20`
- D: `0, 10 and 10, 20`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: C

Regular functions cannot be stopped mid-way after invocation. However, a generator function can be "stopped" midway, and later continue from where it stopped. Every time a generator function encounters a `yield` keyword, the function yields the value specified after it. Note that the generator function in that case doesn’t _return_ the value, it _yields_ the value.

First, we initialize the generator function with `i` equal to `10`. We invoke the generator function using the `next()` method. The first time we invoke the generator function, `i` is equal to `10`. It encounters the first `yield` keyword: it yields the value of `i`. The generator is now "paused", and `10` gets logged.

Then, we invoke the function again with the `next()` method. It starts to continue where it stopped previously, still with `i` equal to `10`. Now, it encounters the next `yield` keyword, and yields `i * 2`. `i` is equal to `10`, so it returns `10 * 2`, which is `20`. This results in `10, 20`.

</p>
</details>

---

###### 8. What's the output?

```javascript
class Dog {
  constructor(name) {
    this.name = name;
  }
}

Dog.prototype.bark = function() {
  console.log(`Woof I am ${this.name}`);
};

const pet = new Dog("Mara");

pet.bark();

delete Dog.prototype.bark;

pet.bark();
```

- A: `"Woof I am Mara"`, `TypeError`
- B: `"Woof I am Mara"`, `"Woof I am Mara"`
- C: `"Woof I am Mara"`, `undefined`
- D: `TypeError`, `TypeError`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A

We can delete properties from objects using the `delete` keyword, also on the prototype. By deleting a property on the prototype, it is not available anymore in the prototype chain. In this case, the `bark` function is not available anymore on the prototype after `delete Dog.prototype.bark`, yet we still try to access it.

When we try to invoke something that is not a function, a `TypeError` is thrown. In this case `TypeError: pet.bark is not a function`, since `pet.bark` is `undefined`.

</p>
</details>

---

###### 9. What's the output?

```javascript
const person = { name: "Lydia" };

Object.defineProperty(person, "age", { value: 21 });

console.log(person);
console.log(Object.keys(person));
```

- A: `{ name: "Lydia", age: 21 }`, `["name", "age"]`
- B: `{ name: "Lydia", age: 21 }`, `["name"]`
- C: `{ name: "Lydia"}`, `["name", "age"]`
- D: `{ name: "Lydia"}`, `["age"]`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: B

With the `defineProperty` method, we can add new properties to an object, or modify existing ones. When we add a property to an object using the `defineProperty` method, they are by default _not enumerable_. The `Object.keys` method returns all _enumerable_ property names from an object, in this case only `"name"`.

Properties added using the `defineProperty` method are immutable by default. You can override this behavior using the `writable`, `configurable` and `enumerable` properties. This way, the `defineProperty` method gives you a lot more control over the properties you're adding to an object.

</p>
</details>

---

###### 10. What's the output?

```javascript
[1, 2, 3, 4].reduce((x, y) => console.log(x, y));
```

- A: `1` `2` and `3` `3` and `6` `4`
- B: `1` `2` and `2` `3` and `3` `4`
- C: `1` `undefined` and `2` `undefined` and `3` `undefined` and `4` `undefined`
- D: `1` `2` and `undefined` `3` and `undefined` `4`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: D

The first argument that the `reduce` method receives is the _accumulator_, `x` in this case. The second argument is the _current value_, `y`. With the reduce method, we execute a callback function on every element in the array, which could ultimately result in one single value. 

In this example, we are not returning any values, we are simply logging the values of the accumulator and the current value.

The value of the accumulator is equal to the previously returned value of the callback function. If you don't pass the optional `initialValue` argument to the `reduce` method, the accumulator is equal to the first element on the first call.

On the first call, the accumulator (`x`) is `1`, and the current value (`y`) is `2`. We don't return from the callback function, we log the accumulator and current value: `1` and `2` get logged.  

If you don't return a value from a function, it returns `undefined`. On the next call, the accumulator is `undefined`, and the current value is `3`. `undefined` and `3` get logged. 

On the fourth call, we again don't return from the callback function. The accumulator is again `undefined`, and the current value is `4`. `undefined` and `4` get logged.
</p>
</details>

---

###### 11. How can we log the values that are commented out after the console.log statement?

```javascript
function* startGame() {
  const answer = yield "Do you love JavaScript?";
  if (answer !== "Yes") {
    return "Oh wow... Guess we're gone here";
  }
  return "JavaScript loves you back ❤️";
}

const game = startGame();
console.log(/* 1 */); // Do you love JavaScript?
console.log(/* 2 */); // JavaScript loves you back ❤️
```

- A: `game.next("Yes").value` and `game.next().value`
- B: `game.next.value("Yes")` and `game.next.value()`
- C: `game.next().value` and `game.next("Yes").value`
- D: `game.next.value()` and `game.next.value("Yes")`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: C

A generator function "pauses" its execution when it sees the `yield` keyword. First, we have to let the function yield the string "Do you love JavaScript?", which can be done by calling `game.next().value`.

Every line is executed, until it finds the first `yield` keyword. There is a `yield` keyword on the first line within the function: the execution stops with the first yield! _This means that the variable `answer` is not defined yet!_

When we call `game.next("Yes").value`, the previous `yield` is replaced with the value of the parameters passed to the `next()` function, `"Yes"` in this case. The value of the variable `answer` is now equal to `"Yes"`. The condition of the if-statement returns `false`, and `JavaScript loves you back ❤️` gets logged.

</p>
</details>

---

###### 12. What's the output?

```javascript
class Person {
  constructor(name) {
    this.name = name
  }
}

const member = new Person("John")
console.log(typeof member)
```

- A: `"class"`
- B: `"function"`
- C: `"object"`
- D: `"string"`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: C

Classes are syntactical sugar for function constructors. The equivalent of the `Person` class as a function constructor would be:

```javascript
function Person() {
  this.name = name
}
```

Calling a function constructor with `new` results in the creation of an instance of `Person`, `typeof` keyword returns `"object"` for an instance. `typeof member` returns `"object"`. 

</p>
</details>

---

###### 13. What's the output?

```javascript
const person = {
  name: "Lydia",
  age: 21
}

for (const [x, y] of Object.entries(person)) {
  console.log(x, y)
}
```

- A: `name` `Lydia` and `age` `21`
- B: `["name", "Lydia"]` and `["age", 21]` 
- C: `["name", "age"]` and `undefined`
- D: `Error`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A

`Object.entries(person)` returns an array of nested arrays, containing the keys and objects:

`[ [ 'name', 'Lydia' ], [ 'age', 21 ] ]` 

Using the `for-of` loop, we can iterate over each element in the array, the subarrays in this case. We can destructure the subarrays instantly in the for-of loop, using `const [x, y]`. `x` is equal to the first element in the subarray, `y` is equal to the second element in the subarray. 

The first subarray is `[ "name", "Lydia" ]`, with `x` equal to `"name"`, and `y` equal to `"Lydia"`, which get logged.
The second subarray is `[ "age", 21 ]`, with `x` equal to `"age"`, and `y` equal to `21`, which get logged.

</p>
</details>

---

###### 14. What's the output?

```javascript
function getItems(fruitList, ...args, favoriteFruit) {
  return [...fruitList, ...args, favoriteFruit]
}

getItems(["banana", "apple"], "pear", "orange")
```

- A: `["banana", "apple", "pear", "orange"]`
- B: `[["banana", "apple"], "pear", "orange"]` 
- C: `["banana", "apple", ["pear"], "orange"]`
- D: `SyntaxError`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: D

`...args` is a rest parameter. The rest parameter's value is an array containing all remaining arguments, **and can only be the last parameter**! In this example, the rest parameter was the second parameter. This is not possible, and will throw a syntax error. 

```javascript
function getItems(fruitList, favoriteFruit, ...args) {
  return [...fruitList, ...args, favoriteFruit]
}

getItems(["banana", "apple"], "pear", "orange")
```

The above example works. This returns the array `[ 'banana', 'apple', 'orange', 'pear' ]`
</p>
</details>

---

###### 15. What's the output?

```javascript
class Person {
  constructor() {
    this.name = "Lydia"
  }
}

Person = class AnotherPerson {
  constructor() {
    this.name = "Sarah"
  }
}

const member = new Person()
console.log(member.name)
```

- A: `"Lydia"`
- B: `"Sarah"`
- C: `Error: cannot redeclare Person`
- D: `SyntaxError`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: B

We can set classes equal to other classes/function constructors. In this case, we set `Person` equal to `AnotherPerson`. The name on this constructor is `Sarah`, so the name property on the new `Person` instance `member` is `"Sarah"`.

</p>
</details>

---

###### 16. What's the output?

```javascript
const info = {
  [Symbol('a')]: 'b'
}

console.log(info)
console.log(Object.keys(info))
```

- A: `{Symbol('a'): 'b'}` and `["{Symbol('a')"]`
- B: `{}` and `[]`
- C: `{ a: "b" }` and `["a"]`
- D: `{Symbol('a'): 'b'}` and `[]`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: D

A Symbol is not _enumerable_. The Object.keys method returns all _enumerable_ key properties on an object. The Symbol won't be visible, and an empty array is returned. When logging the entire object, all properties will be visible, even non-enumerable ones.

This is one of the many qualities of a symbol: besides representing an entirely unique value (which prevents accidental name collision on objects, for example when working with 2 libraries that want to add properties to the same object), you can also "hide" properties on objects this way (although not entirely. You can still access symbols using the `Object.getOwnPropertySymbols()` method).

</p>
</details>

---

###### 17. Is this output truly?

```javascript
const obj = {a: 1, b: 2, c: 3}
console.log(Object.keys(obj))
console.log(Object.values(obj))
console.log(Object.entries(obj))

`[a, b, c]` `[1, 2, 3]` `[['a', '1'], ['b', '2'], ['c': '3']]`
```

- A: true
- B: false

<details>
<summary><b>Answer</b></summary>
<p>

#### Answer: A

These are es6 grammar. `Object.keys` function output `obj` variable's key, `Object.values` function output the value each key from `obj` variable and `Object.entries` function output two-dimensional array key-value make up a array.

</p>
</details>

---

###### 18. What's the output?

```javascript
const getList = ([x, ...y]) => [x, y]
const getUser = user => { name: user.name, age: user.age }

const list = [1, 2, 3, 4]
const user = { name: "Lydia", age: 21 }

console.log(getList(list))
console.log(getUser(user))
```

- A: `[1, [2, 3, 4]]` and `undefined`
- B: `[1, [2, 3, 4]]` and `{ name: "Lydia", age: 21 }`
- C: `[1, 2, 3, 4]` and `{ name: "Lydia", age: 21 }`
- D: `Error` and `{ name: "Lydia", age: 21 }`

<details><summary><b>Answer</b></summary>
<p>

#### Answer: A

The `getList` function receives an array as its argument. Between the parentheses of the `getList` function, we destructure this array right away. You could see this as:

 `[x, ...y] = [1, 2, 3, 4]`

 With the rest parameter `...y`, we put all "remaining" arguments in an array. The remaining arguments are `2`, `3` and `4` in this case. The value of `y` is an array, containig all the rest parameters. The value of `x` is equal to `1` in this case, so when we log `[x, y]`, `[1, [2, 3, 4]]` gets logged.

 The `getUser` function receives an object. With arrow functions, we don't _have_ to write curly brackets if we just return one value. However, if you want to return an _object_ from an arrow function, you have to write it between parentheses, otherwise no value gets returned! The following function would have returned an object:

```const getUser = user => ({ name: user.name, age: user.age })```

Since no value gets returned in this case, the function returns `undefined`.

</p>
</details>

---

###### 19. what's the output?

```javascript
console.log(1)
new Promise ((res, rej) => {
  console.log(2)
  setTimeout(res, 1000)
}).then(() => {
  console.log(3)
})
setTimeout(() => {console.log(4)}, 0)
console.log(5)
```

- A: `1` `2` `3` `4` `5`
- B: `1` `2` `5` `4` `3`
- C: `1` `2` `5` `3` `4`

<details>
<summary><b>Answer</b></summary>
<p>

#### Answer: B

This is the event loop question, about Microtasks and Macro-Tasks. 
<br><img src="https://img-blog.csdn.net/2018041120124254?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xjMjM3NDIzNTUx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70" width="450" /><br>
what is the Microtasks and Macro-Tasks?
<br><img src="https://img-blog.csdn.net/20180411202638415?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xjMjM3NDIzNTUx/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70" width="450" /><br>
`Promise` and `process.nextTick` are the Microtasks. `setTimeout`, `setInterval` and `script` are the Macro-Tasks.
so Macro-Tasks are not executed until Microtasks are completed, but `setTimeout` `0s` is prior to `1000s` executed.
</p>
</details>

---

###### 20. which result is true?

```javascript
(async function(){
  function fnc () {
    return new Promise((res, rej) => {
      setTimeout(res, 5 * 1000)
    })
  }

  `1`
  console.time()
  await fnc()
  await fnc()
  console.timeEnd()

  `2`
  console.time()
  fnc()
  const fn = async function () {
    await fnc()
  }
  await fn()
  console.timeEnd()

  `3`
  console.time()
  fnc()
  const fn = async function () {
    const f = fnc()
    await f
    await fnc()
  }
  await fn()
  console.timeEnd()
})()
```

- A: `10s多` `5s多` `10s多`
- B: `10s多` `10s多` `5s多`
- C: `5s多` `5s多` `10s多`
- D: `5s多` `5s多` `5s多`

<details>
<summary><b>Answer</b></summary>
<p>

#### Answer: A

This is a `async/await` runtime question. The next await function executed until the first one finished, and setTimeout time is 5000s. so example `1` need maybe 10001s, example `2` One synchronous and one asynchronous need time maybe 5001s, and example `3` the asynchronous function `fn` have two asynchronous function so need runtime maybe 10001s.
</p>
</details>

---

###### 21. what's the output?

```javascript
`1`
console.log(parseInt('10'))
console.log(parseInt('121', 3))
console.log(parseInt('1139', 8))
console.log(parseInt('0x1003'))

`2`
['10', '10', '10', '10', '10'].map(parseInt)
```

- A: `10` `13` `NaN` `NaN` and `[10, NaN, NaN, NaN, NaN]`
- B: `10` `16` `75` `4099` and `[10, NaN, 2, 3, 4]`
- C: `10` `16` `NaN` `NaN` and `[10, NaN, 2, 3, 4]`
- D: `10` `13` `NaN` `4099` and `TypeError`

<details>
<summary><b>Answer</b></summary>
<p>

#### Answer: B

This question what you needs understand `parseInt`. At first,  `parseInt` can force change `string` to `number`, and `parseInt` have two
arguments `string` and `radix`. when not _radix_ or _radix_ is 0, default _radix_ is `10`, and when `string` is `0x` or `0X` before, _radix_ is `16`. Each of the string, when value is bigger than _radix_, this value and later are ignored.

In example `1`, the second output is `16`, computing method is `3的0次方 + 3的一次方 * 2 + 3的二次方`. And the third output is `75`.

The other example `2`, `map` have three arguments, `callback` `index` and `source array`. so this example could be understanded:

```javascript
parseInt('10', 0)
parseInt('10', 1)
parseInt('10', 2)
parseInt('10', 3)
parseInt('10', 4)
```
so the result is `[10, NaN, 2, 3, 4]`

</p>
</details>

---

###### 22. Can use `Reflect` object replace `Object` object's internal approach?

- A: true
- B: false

<details>
  <summary><b>Answer</b></summary>
  <p>
  
  #### Answer: A
  
  In the grammar of ECAMScript6, we can use `Reflect` object replace `Object`'s related method. For this example:
  
  ```javascript
  const obj = {}
  
  // when you add a defineProperty for this object variable, you can use this default method.
  Object.defineProperty(obj, 'a', {value: 1})
  
  // but now you can use Reflect object
  Reflect.defineProperty(obj, 'b', {value: 2})  // true
  ```
  
  And use `Reflect` only return true or false, but `Object` maybe throw a error when it hasn't `defineProperty` or another attributes.
  
  </p>
</details>

---

###### 23. what's the output?

```javascript
function exec (type) {
  const v1 = Object.prototype.toString.call(type)
  const v2 = Array.isArray(type)
  const v3 = type instanceof Object
  console.log(v1, v2, v3)
}

exec([])
```

- A: `'array'` `true` `false`
- B: `'[object Array]'` `true` `true`
- C: `'[object Array]'` `true` `false`

<details>
  <summary><b>Answer</b></summary>
  <p>
  
  #### Answer: B
  
  The first `Object.prototype.toString` return a object string type, so `Object.prototype.toString` method can distinguish all types (`Number` `String` `Undefined` `Null` `Array` `Object` `Boolean`). And publish string type form like `"[object String]"`.
  
  And isArray is used to determine whether it's an array, so when afferent an empty array, and it's array type. but `instanceof` operator is used to determine struct function's `prototype` attribute what appear in the prototype chain, so `[] instanceof Array` is true and `[] instanceof Object` is true too.
  
  </p>
</details>
