---
title: Array.prototype.lastIndexOf()
slug: Web/JavaScript/Reference/Global_Objects/Array/lastIndexOf
tags:
  - Array
  - ECMAScript 5
  - JavaScript
  - Method
  - Prototype
  - Polyfill
browser-compat: javascript.builtins.Array.lastIndexOf
---
{{JSRef}}

The **`lastIndexOf()`** method returns the last index at which
a given element can be found in the array, or -1 if it is not present. The array is
searched backwards, starting at `fromIndex`.

{{EmbedInteractiveExample("pages/js/array-lastindexof.html")}}

## Syntax

```js
lastIndexOf(searchElement)
lastIndexOf(searchElement, fromIndex)
```

### Parameters

- `searchElement`
  - : Element to locate in the array.
- `fromIndex` {{optional_inline}}
  - : The position in the array at which to start searching backwards. Defaults to the array's length minus one (`arr.length - 1`), causing the whole array to be searched.

    A `fromIndex` value greater than or equal to the length of the array also causes the whole array to be searched. (In this case, you can think of it conceptually as causing the method to start its search at a nonexistent position beyond the end of the array, but to then go backwards from there looking for the real end position of the array, at which point it starts searching backwards through the actual array elements.)

    A `fromIndex` value greater than 0 is taken as the offset from the beginning of the array.

    A `fromIndex` value less than 0 is taken as the offset from the end of the array — in other words, it is taken as specifying the position at `array.length + fromIndex`. Therefore, if `array.length + fromIndex` is less than 0, the array is not searched, and the method returns -1. (In this case, because `fromIndex` specifies a nonexistent position before the beginning of the array, you can think of it conceptually as causing the method to start its search at that nonexistent position and to then go backwards from there looking for array elements, which it never finds.)

### Return value

The last index of the element in the array; **-1** if not found.

## Description

`lastIndexOf` compares `searchElement` to elements of the Array
using [strict
equality](/en-US/docs/Web/JavaScript/Reference/Operators/Strict_equality) (the same method used by the `===`, or triple-equals, operator).

## Examples

### Using `lastIndexOf`

The following example uses `lastIndexOf` to locate values in an array.

```js
var numbers = [2, 5, 9, 2];
numbers.lastIndexOf(2);     // 3
numbers.lastIndexOf(7);     // -1
numbers.lastIndexOf(2, 3);  // 3
numbers.lastIndexOf(2, 2);  // 0
numbers.lastIndexOf(2, -2); // 0
numbers.lastIndexOf(2, -1); // 3
```

### Finding all the occurrences of an element

The following example uses `lastIndexOf` to find all the indices of an
element in a given array, using {{jsxref("Array.prototype.push", "push")}} to add them
to another array as they are found.

```js
var indices = [];
var array = ['a', 'b', 'a', 'c', 'a', 'd'];
var element = 'a';
var idx = array.lastIndexOf(element);
while (idx != -1) {
  indices.push(idx);
  idx = (idx > 0 ? array.lastIndexOf(element, idx - 1) : -1);
}

console.log(indices);
// [4, 2, 0]
```

Note that we have to handle the case `idx == 0` separately here because the
element will always be found regardless of the `fromIndex` parameter if it is
the first element of the array. This is different from the
{{jsxref("Array.prototype.indexOf", "indexOf")}} method.

## Polyfill

`lastIndexOf` was added to the ECMA-262 standard in the 5th edition; as such
it may not be present in other implementations of the standard. You can work around this
by inserting the following code at the beginning of your scripts, allowing use of
`lastIndexOf` in implementations which do not natively support it. This
algorithm is exactly the one specified in ECMA-262, 5th edition, assuming
{{jsxref("Object")}}, {{jsxref("TypeError")}}, {{jsxref("Number")}},
{{jsxref("Math.floor")}}, {{jsxref("Math.abs")}}, and {{jsxref("Math.min")}} have their
original values.

```js
// Production steps of ECMA-262, Edition 5, 15.4.4.15
// Reference: https://es5.github.io/#x15.4.4.15
if (!Array.prototype.lastIndexOf) {
  Array.prototype.lastIndexOf = function(searchElement /*, fromIndex*/) {
    'use strict';

    if (this === void 0 || this === null) {
      throw new TypeError();
    }

    var n, k,
      t = Object(this),
      len = t.length >>> 0;
    if (len === 0) {
      return -1;
    }

    n = len - 1;
    if (arguments.length > 1) {
      n = Number(arguments[1]);
      if (n != n) {
        n = 0;
      }
      else if (n != 0 && n != (1 / 0) && n != -(1 / 0)) {
        n = (n > 0 || -1) * Math.floor(Math.abs(n));
      }
    }

    for (k = n >= 0 ? Math.min(n, len - 1) : len - Math.abs(n); k >= 0; k--) {
      if (k in t && t[k] === searchElement) {
        return k;
      }
    }
    return -1;
  };
}
```

Again, note that this implementation aims for absolute compatibility with
`lastIndexOf` in Firefox and the SpiderMonkey JavaScript engine, including in
several cases which are arguably edge cases. If you intend to use this in real-world
applications, you may be able to calculate `from` with less complicated code
if you ignore those cases.

## Specifications

{{Specifications}}

## Browser compatibility

{{Compat}}

## See also

- A polyfill of `Array.prototype.lastIndexOf` is available in [`core-js`](https://github.com/zloirock/core-js#ecmascript-array)
- {{jsxref("Array.prototype.indexOf()")}}
- {{jsxref("TypedArray.prototype.lastIndexOf()")}}
