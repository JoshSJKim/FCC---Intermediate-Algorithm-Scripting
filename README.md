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
    const argsArray = Array.from(arguments).slice(1); // or [...arguments].slice(1);
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

## Wherefore art thou

- create a function that iterates through an array of objects (first argument - collection) and returns an array of all objects that have matching key and value pairs (second argument - source).
- Each key and value pair of the source object has to be present in the object from the collection if it is to be included in the returned array.

- This challenge sounds simple enough, but it's a bit complex to iterate through objects in an array and to determine if the source object's key value pair matches the key value pair of the collection array objects.

```js
function whatIsInAName (collection, source) {
  return collection.filter(obj =>
    Object.keys(source).every(key =>
    obj.hasOwnProperty(key) && obj[key] === source[key]));
}
```

- function 'whatIsInAName' takes two arguments - 'collection' and 'source'
- 'collection' is an array of objects. The function should return a new array containing an object from 'collection', which has the items specified 'source'
- The function uses the filter method on 'collection' to iterate through each of the objects in the array.
- It will return a new array containing objects that meet a specified conditions.
- The conditions are defined by the callback function passed to the filter method.
- the callback function takes one parameter, 'obj', which represents the current object being processed in the 'collection' array.
- when iterating over an object in the collection array, Object.keys method is used to return an array of all the 'key' properties from the 'source'.
- 'every' method is used to iterate over this 'key' array, and once again checks to see if every element in the array satisfies certain conditions.
- Another callback function is introduced to define the test conditions on the 'key' array.
- 'obj.hasOwnProperty(key)' checks to see if the current 'key' from the 'source' object is found in the 'obj' from 'collection.
- If true, then it goes on to check if the value of 'key' in the collection 'obj' is strictly equal to the value of 'key' in the 'source' object.
- After iterating through the 'collection' array, if an object containing a key-value pair identical to the key-value pair of 'source' is found, that 'obj' will be included in the new filtered array.

## Spinal Tap Case

- Convert a string spinal case.

```js
function spinalCase(str) {
  let newStr = str.replace(/[\s_]+/g, ' ')
                  .split(/(?=[A-Z])|\s+/)
                  .join('-')
                  .toLowerCase();
  return newStr;
}
```

- It's not necessary to create a new variable for 'newStr'
- But just for clarity sake, I did.
- first replace all whitespace and underscore with an empty whitespace
- use positive lookahead to split string at each occurrence of an uppercase letter or whitespace character
- join split strings with hyphen
- convert joined string to lowercase

- This looks unnecessarily complex.
- Better look for a more logical solution

- I need to learn more about regex expressions.
- Some general rules to keep in mind
  - square brackets ([]) are used to define a 'character class'.
    - It is a set of characters that can match a single character in the input string.
    - '[aeiou]' will match any vowel character.
  - Parentheses '()' are used to define a 'capturing group'.
    - It is a sub-pattern that is matched and captured as a separate group within a larger pattern.
    - The contents of the parentheses can be referenced later in the pattern or in a replacement string.
- In general, use square brackets when you want to match a single character out of a set of characters.
- use parentheses when you want to capture a sub-pattern as a separate group.
- There are other uses for brackets and parentheses, such as defining alterations, lookaheads, and lookbehinds.

- The solution I used earlier seems like there are unnecessary regex patterns that cancel each other out.
  - such as replacing whitespace with a whitespace
  - If I'm going to join the split strings with a '-' later, I don't think I'll need to use the 'replace' method

To simplify

```js
function spinalCase(str) {
  let newStr = str.split(/(?=[A-Z])|[\s_]+/)
                  .join('-')
                  .toLowerCase();
  return newStr;
}
```

- Breakdown the regex portion
  - '(?=[A-Z])' is a positive lookahead assertion that will split the string at each occurrence of an uppercase character
    - This will cover the camelCase strings
  - Use the OR operator (|) to define another condition
  - Use square brackets to define a character class
    - '\s' will split the string at the occurrence of whitespace character
    - '_' will split the string at the occurrence of an underscore
    - '[\s_]+' will split the string at one or more occurrences of whitespace character OR underscore.
      - '+' is not really required for this challenge but I just added it to make it a 'catch-all' expression
- The above regex expression will split the string passed into individual word strings.
- 'join' method is used to join the separated strings with a hyphen '-'
- .toLowerCase() will convert all of the strings to lowercase characters

## Pig Latin

- If a word begins with a consonant, take the first consonant or consonant cluster, move it to the end of the word and add 'ay' to it.
- If a word begins with a vowel, just add 'way' at the end.

```js
function translatePigLatin(str) {
  if (str.match(/^[aeiou]/)) {
    return str.concat('way');
  } else if (str.match(/^[^aeiou]/)) {
    let sliceInd = str.indexOf(str.match(/[aeiou]/)[0]);
    let newStr = str.slice(sliceInd) + str.slice(0, sliceInd).concat('ay');
    return newStr;
  }
}
```

- This is what I initially had.
- It worked but it failed when the string passed doesn't have any vowels, such as 'rhythm'

- Let's first review what I have so far
  - function 'translatePigLatin' takes a string as an argument
  - if the first character of the string is a vowel
    - concatenate string 'way' to the end of the original string and return
  - If the first character of the string is not a vowel
    - create a new variable 'sliceInd' which determines the first occurrence of a vowel in the string and assign its index value to the variable
      - indexOf() is used on the string
        - the string is passed to indexOf() to find a match of the first occurrence of a vowel
          - A LITTLE SIDE NOTE: the [0] is the index operator, which accesses the first element from the array returned from the 'match' method.
            - In the current regex expression (str.match(/[aeiou]/)), we're only looking for the first occurrence of a vowel, so 'match' will return only one element.
            - But 'match' is capable of returning an array of elements if the regex is modified (str.match(/[aeiou]/g)).
            - adding the index operator [0] would be necessary if there are multiple elements in the returned array.
            - Or if you want to access a specific index position from an array of elements.
            - But in the current case, we're only looking for the first occurrence of a vowel in the string, so index operator [0] is not required.
            - You can add it in just to be explicit.
        - 'indexOf' will return the index position of the first vowel occurrence and assign it to 'sliceInd'
      - Declare a new string 'newStr'.
        - slice the original string from 'sliceInd' (first vowel occurrence to the end of the string)
        - concatenate the result of slicing the string from index 0 to 'sliceInd' (the first consonant or consonant cluster) to the end of the string.
        - concatenate string 'ay' to the end of the string.
      - Return 'newStr'

- Now I need to include cases where the string passed to the function does not have any vowels.
- I tried adding the following to the end of the existing code

```js
} else if (str.match(/^[^aeiou]+$/)) { // match string that does not contain any vowels from start to end.
  return str.concat('ay');
}
```

- but it returned an unexpected result, which I can't even explain.
- I think the reason why the existing code does not work for strings without vowels is because of the 'sliceInd'.
  - Since the 'else if' statement checks if the string begins with a consonant, it will go on to look for a vowel in the string
  - It looks for the first occurrence of a vowel, but because it does not find one, it returns 'null'.
  - So it would be difficult to define another regex expression for strings without any vowels since it will always begin with a consonant.
- Then work with the one that I have at the moment.
- Use another 'if' statement within the 'else if' statement
  - if 'str.match(/[aeiou]/)' for 'str.indexOf' does not return null, go ahead with the rest of the execution
  - if 'str.match(/[aeiou]/)' does return null, define another 'else' statement

```js
function translatePigLatin(str) {
  if (str.match(/^[aeiou]/)) {
    return str.concat('way');
  } else if (str.match(/^[^aeiou]/)) {
    let match = str.match(/[aeiou]/);
    if (match !== null) {
      let sliceInd = str.indexOf(match);
      let newStr = str.slice(sliceInd) + str.slice(0, sliceInd).concat('ay');
      return newStr;
    } else {
      return str.concat('ay');
    }
  }
}
```
