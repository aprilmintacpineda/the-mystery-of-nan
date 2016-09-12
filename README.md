# NaN and isNaN in JavaScript
## Foods for thought

###### What is NaN?

**NaN** stands for Not-a-Number, it represents what the name explicitly say, a value that is not a valid number, we can use a built-in function called `isNaN()` to check if a value is NaN.

**Goal:** My goal is to find out how to determine if a value is of NaN value and to create a function that mimics the behavior of `isNaN()`.

Do the following:
- Open new tab
- Right click on the body
- Inspect element
- Click on the **console** tab
- type the following codes one-by-one, you don't have to include the comments

```
isNaN(5); // this will say false
isNaN("5"); // this will say false
isNaN("5a"); // this will say true, because "5a" is not a valid number
isNaN("a"); // this will say true, because "a" is not a valid number
```

The only thing **that I know** which is not equal to itself is the NaN. NaN is of data type `number` therefore if you type `typeof NaN` in the console it would say `"number"`. Aside from **NaN**, there are also some things called **null**, **undefined** but both these things are equal to itself and therefore doing the following will produce the following output:

```
null == null; // would say true
undefined == undefined; // would say true
NaN == NaN; // would say false
```

This is not surprising for experience developer, but as I am writting this, I am just a newbie in JavaScript and it was surprising, it intrigue me to look for an answer for the question "Why is NaN not equal to NaN?" and it raised another question in my head "If NaN is not equal to NaN, how does the `isNaN()` built-in function check if the value given to it is a NaN value?"

```
// a function that would determine if a value is undefined
function isUndefined(a) {
  return a == undefined;
}

// a function that would determine if a value is null
function isNull(a) {
  return a == null;
}

// but to check if a value is NaN we can't just do this
function isNaN(a) {
  return a == NaN; // will always say FALSE
}
```

I asked around and found out that I can do something like this

```
function ifNaN(a) {
  return a !== a;
}

ifNaN(5); // will say false
ifNaN("5"); // will say false

// this behaves almost like isNaN function
isNaN(5); // will say false
isNaN("5"); // will say false

// however,
ifNaN("5a"); // will say false
isNaN("5a"); // will say true

ifNaN("a"); // will say false
isNaN("a"); // will say true
```

this works by determining equality, since NaN is the only thing that is not equal to itself, the function ifNaN would only return true if the parameter given is a NaN value because it would be the same as saying NaN !== NaN which is true.

The solution I came up with is this

```
// function that will determine if something is a NaN value
function ifNaN(a) {
  return a !== a || Number(a) !== Number(a);

  /*
   * returns:
   * false if the value is a valid number
   * true if the value is not a valid number
   */
}

ifNaN(5); // will return false
isNaN(5); // will return false

ifNaN(5.5); // will return false
isNaN(5.5); // will return false

ifNaN("5"); // will return false
isNaN("5"); // will return false

ifNaN("5.5"); // will return false
isNaN("5.5"); // will return false

ifNaN("a"); // will return true
isNaN("a"); // will return true

ifNaN("5a"); // will return true
isNaN("5a"); // will return true

ifNaN(NaN); // will return true
isNaN(NaN); // will return true
```

`a !== a` checks if the value and data type of `a` is not equal to itself, this would be true if we are checking a NaN value and therefore the function will return true. `Number(a) !== Number(a)` first converts the value of a into a number (returning a NaN if it fails) then checks if they are of the same value and data type. Now `condition || condition` would return true if one of these conditions were met and false if both fails and the only way it will fail is if the valud is NaN.