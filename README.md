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

##