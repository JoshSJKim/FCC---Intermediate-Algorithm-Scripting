# FCC---Intermediate-Algorithm-Scripting

## Sum All Numbers in a Range

- Take an array with two numbers.
- Return an array with a range of numbers between the two numbers
- Return the sum of the range of numbers.
- Lowest number will not always come first

- use Math.min() and Math.max() to determine the lowest and highest number
- use a while loop to generate an array with a range of numbers between the numbers passed to the function
- use reduce method to acquire the sum of the range of numbers

```js
function sumAll(arr) {
    const min = Math.min(...arr);
    const max = Math.max(...arr);
    let i = max;
    let newArr = [];
    while (i >= min) {
        newArr.push(i);
        i--;
    }
    return newArr.reduce((sum, num) => sum + num, 0);
}

console.log(sumAll[10, 5]); // The function will generate an array with a range of numbers [10, 9, 8, 7, 6, 5]
                            // Then it will return a sum of the elements of the array, 45
```

## Diff Two Arrays

- compare two arrays and return new array with any items found in only one of the two arrays.
- i.e. return the symmetric difference of the two arrays

```js
function diffArray(arr1, arr2) {
    return arr1.filter(x => !arr2.includes(x)).concat(arr2.filter(x => !arr1.includes(x)));
}
```

- function 'diffArray` takes to arrays as its arguments.
- apply filter method on arr1.
  - x represents the current element being processed
  - the callback function iterates through arr2 and compares against x from arr1 to see if a match is NOT found in arr2.
  - If an element x from arr 1 is not found in arr2, it will return an array with that x value
  - If x from arr1 matches all of the elements of arr2, it will return an empty array
  - concat method is used to merge the returned array with another array.
  - The other array is the exact opposite of what happened above.
  - x from arr2 will be compared against elements of arr1 to look for elements that do not find a match.
  - the returned array is merged with the previous array and returned.

- a new method was introduced to me through this exercise
- ```includes()``` is a built-in JS method that is used to check if an array or string "includes" a specific element or substring.
- It returns a boolean value indicating whether the element or substring is present in the array or string.

```js
array.includes(element, fromIndex);
string.includes(substring, fromIndex);
```

- 'array/string' is the array or string being searched
- 'element/substring' is the element or substring being searched for in the array/string
- 'fromIndex' is an optional parameter that specifies the starting index for the search.
  - If not specified, the search will begin at index 0

- Another thing is that adding '!' negates the expression

## Seek and Destroy

- This exercise introduces the ```arguments``` object.
  - it is a local variable that is automatically available inside all functions.
  - It contains an array-like list of the arguments that were passed into the function when it was called
  - regardless of whether or not those arguments were declared as parameters in the function definition.
  - It can be used to access and manipulate the arguments passed to a function.
  - It has a length property like any other array, and each argument can be accessed using its index

- Note that the 'arguments' object is NOT an actual array, but rather an array-like object
- It can be treated like an array to access its elements using indices.
- But it doesn't allow the use of methods like 'push' or 'pop'.
- However, 'argument' objects can be converted to real arrays using 'Array.from()' or the spread operator ('...')

```js
function myFunc() {
    const argsArray = Array.from(arguments);
    console.log(argsArray);
}

myFunc(1, 2, 3); // [1, 2, 3]
```

or

```js
function myFunc() {
    const argsArray = [...arguments];
    console.log(argsArray);
}
```

- function 'destroyer' will take an array 'arr' as its argument
- But the first argument is followed by one or more arguments.
- The function should remove all elements from the initial array that are the same value as the arguments following the first argument

```js
function destroyer(arr) {
    let newArr = [];
    const argsArray = [...arguments];
    for (let i = 1; i < argsArray.length; i++) {
        if (!arr.includes(argsArray[i])) {
            newArr.push(argsArray[i]);
        }
    }
    return newArr;
}

console.log(destroyer([1, 2, 3, 1, 2, 3], 2, 3));
```

- The above code is what I initially came up with, but it returns an empty array.
- The logic is as follows:
  - when function 'destroyer' is called with an array as its argument
  - new variable 'newArr' is declared with an empty array
  - spread operator is used to create an actual array from the 'arguments' object.
  - for loop is used:
    - 'let i = 1' because the argsArray would be [arr, x, y, z...]. So begin iteration at index 1
    - increase i by 1 for the duration of the length of 'argsArray'
  - If 'arr' passed to the function does not (!) include the value stored at argsArray[i],
  - push that value to newArr
  - When the iteration is done, return 'newArr'

- But this returns an empty array.
- After a lot of thinking, I realized that the logic is reversed.
- The above code is looking for argsArray[i] in the 'arr' array.
  - So in the above example, the first argsArray[i] is '2'.
  - Since it finds 2 in the 'arr' array, nothing is pushed.
  - The same thing with 3.

- I have to reverse the argsArray and 'arr' array

```js
function destroyer(arr) {
    let newArr = [];
    const argsArray = Array.from(arguments).slice(1); // Slice the argsArray so that it removes the 'arr' from the array
    for (let i = 0; i < arr.length; i++) {
        if (!argsArray.includes(arr[i])) {
            newArr.push(arr[i]);
        }
    }
    return newArr;
}

console.log(destroyer([1, 2, 3, 1, 2, 3], 2, 3));
```

- In the above code, argsArray is sliced to return a simple array without the 'arr' array.
- the for loop will now iterate through the 'arr' array from index 0, not 'argsArray'.
- If during iteration, there is an element in 'arr' array that is not found in 'argsArray',
- push that element value to newArr.
- When the iteration is complete, return newArr, which will be [1, 1] in this case.

- This was good practice of using and learning the 'arguments' object.
- The challenge specified that I have to use the 'arguments' object
- But I found out that it can be done without it.

```js
function destroyer(arr, ...args) {
    return arr.filter((element) => !args.includes(element));
}
```

- the code above uses the filter method to create a new array containing all elements of the 'arr' array that pass a certain test.
- The test is whether the current element of the 'arr' array being processed is NOT included in the 'args' array.

- The 'args' parameter is defined using the rest parameter syntax (...args_), which captures the remaining arguments in the form of an array.
  - in the case of the example above, 'destroyer([1, 2, 3, 1, 2, 3], 2, 3) will have 'args' array of [2, 3]. So Simple!
- The callback function passed to the 'filter' method checks whether each element of 'arr' array is not included in the 'args' array.
- The negated 'includes' method is used to check whether the current element (from 'arr' array) is not included in the 'args' array.
- If the test returns 'true' (meaning, it's not included in the args array) it will be included in the new array.
