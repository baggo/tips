# tips
> Tips, hacks and guidelines for optimizing JavaScript modules

Above all else, `baggo` modules should prioritize being fast, compact and simple. Therefore, single variable names and minimal whitespace are  acceptable when writing `baggo`-style code.

Regardless, whitespace has been included in some of the following examples for clarity.

## Optimizing loops
A loop is a block of code that is repeated until a specified condition is fulfilled. The three main methods of looping in JavaScript (and other similar languages) are `for` loops, `while` loops and recursion.

`while` loops focus primarily on readability, and recursion is slow in JavaScript. Therefore, the _only_ kind of loop you should be concerned with when writing `baggo` modules is the `for` loop.

When writing a loop in which the current iteration index `i` is used, most programmers will write a `for` loop like this:

```javascript
// for (initial; condition; iteration);
for (var i = 0; i < array.length; i++) {
  var item = array[i]
  // Do stuff with item...
}
```

Unless `array.length` is constantly changing, it is preferable to cache it in a variable to avoid unnecessary property lookups.

```javascript
// In this example, `array.length` is only accessed once
for (var i = 0, max = array.length; i < max; i++) {
  // ...
}
```

An even faster and more compact approach would be to loop backwards through the array:

```javascript
for (var i = array.length; i--;) {
  // ...
}
```

Other than being fast, this method has an additional perk - if you need to remove elements as you iterate, looping backwards may eliminate the need to implement additional low-level logic to accommodate item removal.

It's also possible to move all the code between the braces into the final `iteration` expression of the `for` loop if need be. However, note that common keywords and tokens such as `if` and `break` cannot be used in the middle of expressions. For complex logic, use [conditional and logical expressions](#condensing-conditionals) instead.

After moving all the code out of a loop block, you may use a semicolon to convert the resulting empty block into an empty expression:

```javascript
// Empty block `{}`
for(;;){}

// ...becomes an "empty expression":
for(;;);
```

The semicolon is required at the end of the parentheses if it is followed by an expression that you wish to keep "out of the loop".

```javascript
// `return` statement part of `for` loop, will break out of function on first iteration
for(var r='';...;...)return r

// `return` occurs after the `for` loop (desired result)
for(var r ='';...;...);return r
```

**Note:** Even if an expression in a `for` loop is empty, both semicolons are still required or else the compiler will throw a `SyntaxError`. This is one of the reasons why these kinds of loops struggle with readability.

**Another one:** `for(;;)` is the smallest infinite loop possible.

## Chaining variable declarations
Defining variables in separate declarations can help with readability, but the definitions can also be placed onto one line to save space.

```javascript
// Sequence of variable declarations
var i = 0
var lorem = 'ipsum'
var foo

// ...becomes a "declaration sequence":
var i=0,l='ipsum',f
```

Incidentally, this method fits in naturally into the `initial` expression of `for` loops since keywords like `var` cannot be used in sequence expressions.

## Condensing conditionals
Most `if` statements can be condensed into conditional expressions or logical expressions, allowing you to use them in `for` loops.

```javascript
// Generic `if` statement
if (condition) {
  consequent()
}

// ...becomes a "logical expression":
condition&&consequent()
```

```javascript
// `if`-`else` statement
if (condition) {
  consequent()
} else {
  alternate()
}

// ...becomes a "conditional expression":
condition?consequent():alternate()
```

```javascript
// `if` statement with multiple expressions
if (condition) {
  foo()
  bar()
  baz()
}

// ...becomes a "sequence expression":
condition&&foo(),bar(),baz()
```

## Avoiding `Array` built-ins

To fill up an array with values within a for loop, you can use the current iteration index `i` to set values directly instead of using `Array.push`.

```javascript
var result = []
var i = value.length
while (i--)
  result[i] = FOO
return result

// ...becomes the following, when compressed:
for(var r=[],i=v.length;i--;r[i]=FOO);return r
```

This may or may not impact performance, but it usually just saves some space.
