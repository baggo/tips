# tips

In general, your goal is to convert as many [statements](https://github.com/estree/estree/blob/master/es5.md#statements) as you can into [expressions](https://github.com/estree/estree/blob/master/es5.md#expressions).

## Reuse Old Variables

Create a few "`var` chains" and tag on extra declarations:

```js
var x=123,y=345,z,a,b
```

This way, you don't have to use `var` later on.

**Note:** Be careful with scoping.  Use multiple variables to get past conflicts.

## Smallest Loop

```js
for(init;condition;body);
```

For infinite loops just leave them empty:

```js
for(;;);
```

## Empty expressions

Instead of empty blocks:

```js
while (condition) {}
```

Create "empty expressions":

```js
while(condition);
```

## Blocks to sequence expressions

When normally writing code, you'd do something like this:

```js
if (condition) {
  foo()
  bar()
  baz()
}
```

These can be changed to a "sequence expression":

```js
if(condition)foo(),bar(),baz() 
```

## If statements to conditional expressions

An `if` (with no `else`):

```js
if (condition) {
  consequent()
}
```

Can be changed to:

```js
condition&&consequent()
```

And one with `else`:

```js
if (condition) {
  consequent()
} else {
  alternate()
}
```

Can be changed into a conditional expression:

```js
condition?consequent():alternate()
```

**Note:** Be wary of return values of the expressions, as they can mess up execution depending where used.
