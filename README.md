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

## Search and Replace

- Perform a search and replace on a string
- the function will receive three arguments
  - The string
  - The target word
  - The replacement word
- If the target word's first character is uppercase, so should the replacement
- If the target word's first character is lowercase, so should the replacement

```js
function myReplace(str, before, after) {
  if (before.match(/^[a-z]/) && after.match(/^[A-Z]/)) {
    let afterLower = after.charAt(0).toLowerCase() + after.slice(1);
    return str.replace(before, afterLower);
  } else if (before.match(/^[A-Z]/)) {
    let afterUpper = after.charAt(0).toUpperCase() + after.slice(1);
    return str.replace(before, afterUpper);
  } else {
    return str.replace(before, after);
  }
}
```

- If both second and third arguments are lowercase, the first 'if' statement is not necessary
- But if the second argument is lowercase and third argument is uppercase, it changes things.
- I initially didn't have the first 'if' statement.
- The 'else' statement was my first 'if' statement.

Let's break it down

- function 'myReplace' receives three arguments: string, target word, replacement word.
- If the first character of 'before' is lowercase and the first character of 'after' is uppercase
  - create a new variable 'afterLower', which will convert the character at index 0 of 'after' to lowercase and concatenate with 'after' sliced from index 1
  - return the result of replacing 'before' with 'afterLower'
- If the first character of 'before' is uppercase
  - create a new variable 'afterUpper', which will convert the character at index 0 of 'after' to uppercase and concatenate with 'after' sliced from index 1
  - return the result of replacing 'before' with 'afterUpper'
- In all other cases, return the result of replacing 'before' with 'after'.

- looking at it in retrospect, I don't need to find out if 'after' begins with an uppercase character
- 'after' simply needs to follow the letter casing of 'before'

```js
function myReplace(str, before, after) {
  if (before.match(/^[a-z]/)) {
    let afterLower = after.charAt(0).toLowerCase() + after.slice(1);
    return str.replace(before, afterLower);
  } else if (before.match(/^[A-Z]/)) {
    let afterUpper = after.charAt(0).toUpperCase() + after.slice(1);
    return str.replace(before, afterUpper);
  }
}
```

- I somehow have a feeling that this can be simplified even more, even with my limited knowledge
- If I just check whether 'before' begins with an uppercase or lowercase once, since it's either or, I don't need to check it twice.

```js
function myReplace(str, before, after) {
  if (before.match(/^[a-z]/)) {
    let afterLower = after.charAt(0).toLowerCase() + after.slice(1);
    return str.replace(before, afterLower);
  } else {
    let afterUpper = after.charAt(0).toUpperCase() + after.slice(1);
    return str.replace(before, afterUpper);
  }
}
```

- I know that I don't need to create new variables 'let afterLower' and 'let afterUpper'.
- I can just reassign the variables according to its uppercase or lowercase needs.
- But I think the above code is more readable and easier to understand.

```js
function myReplace(str, before, after) {
  if (before.match(/^[a-z/]/)) {
    after = after.charAt(0).toLowerCase() + after.slice(1);
  } else {
    after = after.charAt(0).toUpperCase() + after.slice(1);
  }
  return str.replace(before, after);
}
```

- If first character of 'before' is lowercase,
  - modify 'after' to begin with lowercase
- If not (i.e. it begins with uppercase)
  - modify 'after' to begin with uppercase
- return the result of replacing 'before' with 'after', accordingly

- Now that I look at it, this is more concise and easy to read.

## DNA Pairing

- Pairs of DNA strands consist of nucleobase pairs.
- Base pairs are represented by the characters AT and CG, which form building blocks of the DNA double helix.
- The DNA strand is missing the pairing element.
- Write a function to match the missing base pairs for the provided DNA strand.
- For each character in the provided string, find the base pair character.
- Return the results as a 2d array.

```js
function pairElement(str) {
  let arr = str.split("");
  let resultArr = [];
  for (let i = 0; i < arr.length; i++) {
    let subArr = [];
    switch (arr[i]) {
      case 'G':
        subArr.push(arr[i], 'C');  // These can be made more explicit by replacing arr[i] with the actual value of arr[i] passed to the case
        break;
      case 'C':
        subArr.push(arr[i], 'G');
        break;
      case 'A':
        subArr.push(arr[i], 'T');
        break;
      case 'T':
        subArr.push(arr[i], 'A');
        break;
    }
    resultArr.push(subArr);
  }
  return resultArr;
}
```

- function 'pairElement' takes a string 'str', which represents base pairs of DNA strands
- declare new variable 'arr' to split the original string into an array of single character elements
- create a new variable 'resultArr' with an empty array, which will be the encapsulating array at the end of the code
- Generic 'for' loop to iterate through the split array 'arr'
  - initialize an empty array 'subArr', to which the DNA pairs will be pushed.
  - It's always confusing when it comes to placing an empty array inside or outside the 'for' loop.
    - If the subArr is placed outside the loop, the loop will push all the pairs into the subArr rather than having separate pairs for each base pair
    - If placed inside the loop, it will push one result into subArr, then break.
    - It will continue to do so for the length of the array, creating a new subArr for each iteration.
- It's been a while since I used a switch statement and I needed a refresher.
  - The switch statement will take the element under processing as its argument and return the appropriate result.
  - When a match is identified, it will push the current arr[i] and its corresponding pair into subArr, then break, and continue iteration.
- I first made the mistake of entering ```return resultArr.push(subArr)```
  - This will terminate the function after the first iteration of the 'for' loop
- push subArr to resultArr until the end of the iteration
- Then return the resultArr.

## Missing Letters

- Find the missing letter in the letter range passed and return it
- If all letters are present, return undefined

- A lot of trial and error on this one.
- Couple of new methods were introduced in this challenge
  - ```string.charCodeAt()```
    - This method returns the Unicode value of the character at a specified index in a string
    - It only works on strings. Note that it also works on string elements in an array
  - ```String.fromCharCode()```
    - This is the opposite of ```.charCodeAt()```.
    - It returns the character corresponding to the unicode value.

```js
function fearNotLetter(str) {
  let arr = str.split("");
  let unicodeArr = arr.map(elem => elem.charCodeAt(0));
  for (let i = 0; i < unicodeArr.length - 1; i++) {
    if (unicodeArr[i] + 1 !== unicodeArr[i + 1]) {
      let missingVal = String.fromCharCode(unicodeArr[i] + 1);
      return missingVal;
    }
  }
  return undefined;
}
```

- function 'fearNotLetter' receives a string, which is a range of letters.
- create an array of individual character strings using split method on 'str'
- convert that array of letters to an array of Unicode values using '.charCodeAt' method
  - 'elem => elem.charCodeAt(0)' passes each element of 'unicodeArr' and returns the unicode value at index 0 of each element
  - since it's a single letter string, it does't really matter in this case, but the '(0)' represents the index value of the character in a string
- use a 'for' loop to iterate through 'unicodeArr'
- if value of 'unicodeArr[i]' plus 1 is not equal to the value of 'unicodeArr[i + 1]'
  - for example, if the array consists of [97, 98, 99, 101]
    - and if the current element in iteration is 99 (unicodeArr[i])
    - the if statement will take 99 + 1 (100) and compare it to the element at the next index (unicodeArr[i + 1]), which is this case is 101
    - Since 100 !== 101, this means that Unicode value '100 is missing from the array.
    - Continue to execute the code in the if statement.
- Use ```String.fromCharCode()``` to retrieve the character corresponding to the missing Unicode value and assign it to a new variable 'missingVal'
- If the 'if' statement returns false throughout the iteration of the array, return 'undefined'

- This seems more inefficient than it needs to be
- I don't need to declare a new variable 'missingVal'
  - I could simply 'return String.fromCharCode(unicodeArr[i] + 1)
- I don't need to declare a new variable 'unicodeArr'
  - simply reassign arr with the result of using map on the array
  - or even put string and map together

```js
function fearNotLetter(str) {
  let arr = str.split("").map(elem => elem.charCodeAt(0));
  for (let i = 0; i <arr.length; i++) {
    if (arr[i] + 1 !== arr[i + 1]) {
      return String.fromCharCode(arr[i] + 1);
    }
  }
  return undefined;
}
```

## Sorted Union

- Write a function that takes two or more arrays and returns a new array of unique values in the order of the original provided arrays.
- In other words, all values present from all arrays should be included in their original order, but with no duplicates in the final array.
- The unique numbers should be sorted by their original order, but the final array should not be sorted in numerical order.

- Had to do a bit of googling to get through this.
- I'm still not used to using most of the methods that I've already encountered.
- And there were more useful tools introduced.

Arguments Object

- In JS, the ```arguments``` object is a local variable available inside every function
- It contains an array-like list of arguments passed to the function when it was called.
- It is automatically created whenever a function is called.
- It contains an entry for each argument passed to the function.
- There are several properties and methods that can be used to access and work with the arguments passed to a function.
- Some commonly used are:
  - ```arguments.length``` The number of arguments passed to a function
  - ```arguments[i]``` The value of an argument at index [i]
  - ```[...arguments]``` Spread the arguments into an array

- ```arguments``` is not an array, but an array-like object.
- So it does not have all the methods of a regular array.

reduce()

- The ```.reduce()``` method is used to reduce an array of values to a single value by iterating over each element in the array.
- It performs a specified operation during iteration.

- It takes two arguments.
  - The callback function
    - The callback also takes two arguments
      - The accumulator value (the result of the previous iteration, throughout the length of the array)
      - And current element being processed
    - The callback function returns the updated accumulator value as it iterates, which is used as the accumulator value for the next iteration.
  - And an optional initial value
    - If not specified, the first element of the array passed to the function will be the initial value.
    - In this case, the first iteration will begin on the second element.

```js
let numbers = [1, 2, 3, 4, 5];
let sum = number.reduce((accumulator, currentValue) => {
  return accumulator + currentValue;
}, 0);
```

- The above is the longhand. Below is the shorthand

```js
let numbers = [1, 2, 3, 4, 5];
let sum = number.reduce((accumulator, currentValue) => accumulator + currentValue, 0);
```

- curly braces are not necessary if the callback function consists of a single expression.
- If the callback function contains multiple statements, or needs to do more complex operations, curly braces are required to create a block of code for the function body.

Set

- ```Set``` is a JS object that represents a collection of unique values.
- It can be used to store any type of value, such as strings, numbers, or objects and ensure that there are no duplicate values in the collection

```js
const mySet = new Set();
```

- The above is the syntax for creating a new 'Set' object
- You can use multiple methods with .Set() once it's created

```js
mySet.add();    // add a value
mySet.delete(); // delete a value
mySet.has();    // check if the set object has a specified value
mySet.size();   // check the number of values in the set object
```

- Now to get back to the challenge

```js
function uniteUnique(arr) {
  let args = [...arguments];
  arr = args.reduce((acc, val) => {
    return acc.concat(...val);
  }, []);
  let uniqueArr = [...new Set(arr)];
  return uniqueArr;
}
```

- function 'uniteUnique' receives an array 'arr' as its argument. Note that 'arr' can represent any number of arrays as arguments
- create a new variable 'args' and assign the result of spreading the 'arguments' in an array
- The the args array containing x number of subarrays need to be merged into a single array.
- reassign 'arr' with the result of using the reduce method on 'args'.
  - The callback function takes two arguments: the accumulator and the current element being processed. In this case, each subarray 'val'
  - arrow function is used to explicitly return the result of concatenating each element of each subarray (...val) as it iterates.
  - the accumulator will gather all elements and .concat method will return the result in a single array.
  - The initial value of the accumulator is explicitly defined as an empty array.
- create a new variable 'uniqueArr'
  - Assign the result of creating a new 'Set' object
  - This removes any duplicate elements in the 'arr' array and stores the unique elements in a new 'Set' object.
  - the spread operator (...) will spread the elements back into 'uniqueArr' and return the resulting array.

- The above code can be more concise using the implicit method for the callback

```js
function uniteUnique(arr) {
  let args = [...arguments];
  arr = args.reduce((acc, val) => acc.concat(...val));
  let uniqueArr = [...new Set(arr)];
  return uniqueArr;
}
```

- The implicit method, or the 'shorthand' syntax does not require the curly braces or the 'return' keyword, as long as the return contains a single expression.
- You can add the initial value of the accumulator (which was an empty array) explicitly, but since the concat method returns an array, it is not really necessary.

## Convert HTML Entities

- Convert the characters &, <, >, " (double quote), and ' (apostrophe), in a string to their corresponding HTML entities.

```js
function convertHTML(str) {
  for (let i = 0; i < str.length; i++) {
    switch(str[i]) {
      case '&': str = str.replace('&', '&amp;');
        break;
      case '<': str = str.replace('<', '&lt;');
        break;
      case '>': str = str.replace('>', '&gt;');
        break;
      case '"': str = str.replace('"', '&quot;');
        break;
      case "'": str = str.replace("'", '&apos;');
    }
  }
  return str;
}
```

- Coming up with my initial solution was not that difficult.
- function 'convertHTML' receives a string 'str' as its argument.
- Iterate through the string using a 'for' loop.
- Use a switch statement to check for specific characters.
- If specified character is found, replace the character with the corresponding HTML entity using replace
- break and return string.

- But I am thinking that there must be a more concise solution.
- I could use regex to identify the HTML entities and create an object defining the HTML entities to its corresponding characters.

```js
function convertHTML(str) {
  const htmlEntities = {
    '&' : '&amp;',
    '<' : '&lt;',
    '>' : '&gt;',
    '"' : '&quot;',
    "'" : '&apos;'
  };
  return str.replace(/[&<>"']/g, elem => htmlEntities[elem]);
}
```

- function 'convertHTML' receives a string 'str' as its argument
- object 'htmlEntities' is created with the target characters and its corresponding HTML entity
- Use the 'replace' method on the string
  - first argument is the target character, defined using regex, enclosed in square brackets to create a character class, with a global flag (g) to match multiple occurrences.
  - second argument is the current element being processed.
    - if a character matching the regex expression is identified, it will return the corresponding value of that element from 'htmlEntities' object and replace.
- the result of the 'replace' method is returned.

## Sum All Odd Fibonacci Numbers

- What are Fibonacci numbers?
  - Fibonacci numbers are a sequence of numbers in which each number after the first tw is the sum of the two preceding numbers.
  - The first two numbers of the sequence is predefined. 0 and 1
    - F(0) = 0
    - F(1) = 1
    - F(n) = F(n-1) + F(n-2) for n > 1
    - (n) represents the nth Fibonacci number in the sequence

- Create a function that receives a positive integer 'num' as its argument
- Return the sum of all odd numbers in the sequence that are less than or equal to 'num'.

- First create a function that generates a sequence of Fibonacci numbers less than or equal to 'num'
- use 'remainder' (%) operator to find odd numbers in the sequence and push to a new array
- return the sum of the array elements (maybe use reduce method?)

```js
function sumFibs(num) {
  let fiboArr = [0, 1];
  for (let i = 2; i <= num; i++) {
    let nextFib = fiboArr[i-1] + fiboArr[i-2];
    if (nextFib > num) {
      break;
    }
    fiboArr.push(nextFib);
  }
  return fiboArr.filter(elem => elem % 2 == 1)
                .reduce((acc, val) => acc + val, 0);
}
```

- It took me a while to understand how to write a code that generates the Fibonacci sequence
- There must be numerous ways to go about this, but I decided to declare a Fibonacci sequence array 'fiboArr' with the first two numbers of the sequence in an array.
- The 'for' loop is what threw me off the most.
  - Up to this date, I've always used the 'for' loop with the 'i' value starting at 0.
  - Because of this, I always thought that the initial variable always referred to the index position of an array.
  - This is true if 'i = 0'
  - And I thought that if I initialize 'let i = 2', the iteration will begin at index position 2.
  - but what threw me off was 'i <= num'
  - The solution above will loop until the counter variable 'i' reaches 'num' number of elements in the array.
  - `I'm starting to understand how the above code works as I am breaking it down`
- Initialize 'nextFib' with the result of the sum of values at `fiboArr[i - 1] + fiboArr[i - 2]`
- if `nextFib` value is greater than `num`, terminate the loop
  - This is the reason why the code passes.
  - This `if` statement ensures that the maximum element value in the array does not exceed the `num` value.
- Push the `nextFib` value to `fiboArr` array throughout the length of the loop.
- This will generate the Fibonacci sequence.
- Now, filter all elements that have odd values using the remainder operator
- Then use the reduce method to acquire the sum of all odd numbers.

- Looking at it now, I can simplify the code a bit by changing the 'less than or equal to' portion of the `for` loop to explicitly define the maximum element value rather than the maximum number of elements.
- If I do that, I could get rid of the if statement that terminates the loop when `nextFib` is greater than `num`.
- I could use the remainder operator to extract the odd numbers during the loop and push directly into the array and acquire the sum at once.

```js
function sumFibs(num) {
  let fiboArr = [0, 1];
    for (let i -= 2; fiboArr[i-1] + fiboArr[i-2] <= num; i++) {
      let nextFib = fiboArr[i-1] + fiboArr[i-2];
      fiboArr.push(nextFib);
    }
    return fiboArr.filter(elem => elem % 2 == 1)
                  .reduce((acc, val) => acc + val, 0);
  }
```

- It didn't quite workout the way I had it planned out in my head.
- Explicitly defining the 'less than or equal to' portion of the `for` loop did allow me to get rid of the `if` statement.
- But I was not able to extract odd numbers only into a single array.
- I think this is trickier because I am not iterating through an existing array.
  - I am creating a new array as the loop runs.
- I tried creating another separate array for the odd numbers, but it would be missing the first two elements (0, 1).
- Also, since `i` is 'tied up' to `fiboArr` and `nextFib`, there's a lot of confusion in the code.
- I honestly can't explain this. It's beyond me.
- Also, creating another array for odd numbers and trying different things made the code even more complex.

- I did some more thinking about this challenge.
- The solution may not be more concise than the one I have, but it is another solution.
- It is essentially the same as the one above, but with a little twist in the for loop.

```js
function sumFibs(num) {
  let fiboArr = [0, 1];
  let oddArr = [0, 1];
  for (let i = 2; fiboArr[i-1] + fiboArr[i-2] <= num; i++) {
    let next Fib = fiboArr[i-1] + fiboArr[i-2];
    if (nextFib % 2 == 1) {
      oddArr.push(nextFib);
    }
    fiboArr.push(nextFib);
  }
  return oddArr.reduce((acc, val) => acc + val, 0);
}
```

OR

```js
function sumFibs(num) {
  let fiboArr = [0, 1];
  let oddSum = 1;
  for (let i = 2; fiboArr[i-1] + fiboArr[i-2] <= num; i++) {
    let next Fib = fiboArr[i-1] + fiboArr[i-2];
    if (nextFib % 2 == 1) {
      oddSum += nextFib;
    }
    fiboArr.push(nextFib);
  }
  return oddSum;
}
```

- The above two solutions separate the odd numbers from fiboArr.
- It is the same as the initial solution up to the 'if' statement.
- If the remainder of the current 'nextFib' is 1 (meaning, if it is an odd number)
  - push that `nextFib` to the separate array `oddArr`, which is also initialized with the same initial array as `fiboArr`
  - `oddArr` will only accumulate odd numbers
- At the same time, push all `nextFib` to `fiboArr`.
  - I noticed that if I don't 'send' the rest of the `nextFib` anywhere, it returns unexpected results.
- Finally, use `reduce` method on `oddArr` to get the sum of the odd numbers.

- The following solution is also the same as the initial solution up to the `if` statement.
- If the remainder of the current `nextFib` is 1 (odd number)
  - continue to add `nextFib` to the accumulating `oddSum` value.
- push all `nextFib` value to `fiboArr` to avoid unexpected results.
- return `oddSum`

## Sum All Primes

- Rewrite sumPrimes so it returns the sum of all prime numbers that are less than or equal to num.

- A prime number is a whole number greater than 1 with exactly two divisors: 1 and itself.
- For example, 2 is a prime number because it is only divisible by 1 and 2.
- In contrast, 4 is not prime since it is divisible by 1, 2 and 4.

- I had to do some googling to find a code that generates an array of prime numbers.
- First, create a function that checks if a given number is prime or not.

```js
const isItPrime = (n) => {
  for (let i = 2; i <= n/2; i++) {
    if (n % i === 0) {
      return false;
    }
  };
  return true;
};
```

- function `isItPrime` receives an integer `n` as its argument
- the function returns `true` if the number passed is a prime number and `false` if it is not.
- It uses a `for` loop that iterates over all numbers between the initial `2` and upper limit `n/2`, inclusive
  - This is to check if `n` is divisible by each number iterated.
  - A number cannot be divided by any number that is greater than 1/2 of its value.
  - If `n` is divisible by any number in the range, it means that it is not a prime number and the function returns `false`.
  - If none of the iterated integer(s) could divide `n`, it means that `n` is a prime number and the function returns `true`.

- Now we need a function that receives an integer as its argument and generate an array comprised of prime numbers less than or equal to the argument value
- Then the prime numbers will be iterated to return a sum value.

```js
const isItPrime = n => {
  for (let i = 2; i <= n/2; i++) {
    if (n % i === 0) {
      return false;
    }
  }
  return true;
};
const sumPrimes = num => {
  let primeArr = [];
  let i = 2;
  while(i <= num) {
    if(isItPrime(i)) {
      primeArr.push(i);
    }
    i = (i % 2 === 0) ? i + 1 : i + 2;
  }
  return primeArr.reduce((acc, val) => acc + val, 0);
};
```

- function `sumPrimes` receives an integer `num` as its argument.
- `primeArr` is initialized with an empty array
- `i` is initialized with a value of `2`, since it is the first prime number
- while `i` is less than or equal to `num`
  - if `isItPrime` returns `true` for the value of `i` passed to the function,
  - push `i` to `primeArr` array.
- using the ternary operator,
  - the value of `i` increments by 1 if the current value of `i` is an even number
  - otherwise increments by 2
  - This is to ensure that the loop only iterates through odd numbers for code optimization and efficiency
- The `while` loop continues to loop until the value of `i` reaches the value of `num`
- Once the loop terminates and `primeArr` is complete, use the `reduce` method to acquire the sum of the acquired prime numbers

- Google did most of the work. It took a while for me to understand the logic of the code.

## Smallest Common Multiple

- Find the smallest common multiple of the provided parameters that can be evenly divided by both
- as well as by all sequential numbers in the range between these parameters.
- The range will be an array of two numbers that will not necessarily be in numerical order.

- Without giving it too much thought, this is what I initially came up with

```js
function smallestCommons(arr) {
  arr = arr.sort((a, b) => a - b);
  let range = [];
  for (let i = arr[0]; i <= arr[arr.length-1]; i++) {
    range.push(i);
  };
  let num = range[range.length-1];
  let m = 1;
  for (let j = num; j > 0; j--) {
    if ((num * m) % j !== 0) {
      m++;
    }
  }
  return num * m;
}
```

- Since the argument 'arr' passed to the function may not be in numerical order, use the sort method to rearrange the arguments in ascending order.
- initialize a new array 'range' with an empty array.
- use a `for` loop to create a range of integers between the lowest number to the highest number, inclusive.
- That was the easy part.
- initialize new variable `num` with the value of the last element in the newly created `range` array.
- initialize a multiplier `m` with the value 1
- Use a reverse `for` loop to iterate from the highest value `j` of the `range` array down to 0.
- As it iterates through each element, check if the `num` value multiplied by the `m` is divisible by `j`
- If false, increment `m` by 1.
- If true until `j` value reaches 1, return `num * m`.

- That was what I was hoping to achieve, but the code is not constructed the way I want it to.
- I think the problem is the second `for` loop. It just feels like something is missing.
- I want the code to check if num * m value is divisible by the entire range.
- But the flaw in the above code is that `j` will not reset to `range[range.length-1]` when `m` value increments
- I need to change the code so that
  - if `(num * m)` is not divisible by the current number in the range `j`, it should stop and move on to the next value of `j` in the range, instead of incrementing `m`.
  - `m` should not increment until the entire `range` array is checked against `num * m`.
  - In other words, the `for` loop should be reset when `m` increments. In other words, `m++` should be outside the `for` loop
  - Does that mean I need to put the `for` loop inside another loop?
  - I'm used to using loops with an upper or lower limit - a definite termination point.
  - How do I use a loop (potentially infinite) until a certain condition is satisfied?
  - The condition in this case is `if((num * m) % j === 0)` for all of the elements of the `range` array.
  - So I just need to nest that `for` loop and `if`statement within another loop.

- Googled 'infinite loop in JS' and the first thing that came up was `while(true)`.
- It is one of the most common methods to create an infinite loop.

```js
function smallestCommons(arr) {
  arr = arr.sort((a, b) => a - b);
  let range = [];
  for (let i = ar[0]; i <= arr[arr.length-1]; i++) {
    range.push(i)l
  };
  let num = range[range.length-1];
  let m = 1;
  while(true) {
    for (let j = 0; j < range.length; j++) {
      if ((num * m) % range[j] !== 0) {
        break;
      }
      return num * m;
    }
    m++; 
  }
}
```

- Tried the above, but it obviously doesn't make any sense.
- I changed the `for` loop to be in ascending order since there is no evident reason that it has to be reversed.
- Also, I changed `j` in the `if` statement to `range[j]` because `num * m` needs to be checked against the value of the element at index [j] of the `range` array, not the index position of the array.
- if if `num * m` is not divisible by `range[j]`, break the for loop.
- but it returns `num * m`, which doesn't make sense.
- `m++` is outside the `for` loop so that if the entire range of `range` array does not successfully divide `num * m`, the `m` value will increment by 1 and repeat the `for` loop process again.
- But the above code does not have any way to return `num * m` only if a successful match is found.
- The above loop will stop as soon as it starts and return `num * m`

- I need the code to:
  - break the `for` loop during iteration if `num * m` is not divisible by the current number in the iteration.
  - Then it should move on to the next value of `m`.
  - but if a match is found, there is no need to increment, so if a match is found, it should immediately return `num * m` and terminate the potentially infinite loop.
  - In other words, if a match is found, return `num * m` before `m++`. So I need another if statement.

- Did some more googling and found `flag variables`. It is so rudimentary that I didn't even think about it.
  - `flag variable` is a variable you define to have one value until SOME CONDITION IS SATISFIED, in which case you change the variable's value.
  - It is a variable available to control the flow of the function statement, allowing you to check for certain conditions while the function executes.

- So use flag variables to define if false, do this, and if true, do this.
  - In the case above, if false, break `for` loop and m++ and repeat.
  - if true, return `num * m`.
  - Flag variable should be set to true outside the `for` loop but inside the `while` loop so that the `while` loop runs as long as the flag variable is `true`
  - It should change its value to false if the `if` statement returns a `true` boolean value. In other words, if `num * m` is NOT divisible by `range[j]`, the statement's boolean value would be `true`, which should trigger the flag variable value to change to `false`, break the `for` loop, increment `m` and repeat process.
  - If the flag variable remains `true` throughout the entire range of the `range` array, (in other words, `num * m` is divisible by all of the elements of the `range` array and never triggers the flag variable to change value to `false`), return `num * m` BEFORE `m++`

- In code

```js
function smallestCommons(arr) {
  arr = arr.sort((a, b) => a - b);
  let range = [];
  for (let i = arr[0]; i <= arr[arr.length - 1]; i++) {
    range.push(i);
  };
  let num = range[range.length - 1];
  let m = 1;
  while(true) {
    let lcm = true;
  for (let j = 0; j < range.length; j++) {
    if((num * m) % range[j] !== 0) {
      lcm = false;
      break;
    }
    } if(lcm) {
      return num * m;
    }
    m++;
  }
}
```

- function 'smallestCommons` takes an array with two numbers as its argument.
- The order of the two numbers may not be in order, so use a simple `sort` method to rearrange the array in ascending order.
- Use a for loop to acquire an array with a range of integers between the two initial values, inclusive.

- initialize `num` with the largest value of the element in `range`array.
- initialize a multiplier `m` with a value of 1.
- Use an infinite `while` loop to run until desired result is identified.
- initialize a flag variable 'lcm' (Least Common Multiple) and set its value to `true`. The while loop will continue as long as `lcm` is true.
- use a `for` loop to iterate through the `range` array.
- During iteration, check if `num * m` is divisible by the value of the element at index [j] of the range array.
- if `num * m` is NOT divisible by a certain number in the `range` array, it means that `num * m` is not the smallest common multiple.
- change value of 'lcm' to false and break the `for` loop.
- since 'lcm' is `false`, skip the next `if` statement and increment `m++` to repeat the `for` loop process.
- At this point, `lcm` is reset to `true`, and the `for` loop is also reset.
- If the `if` statement does not trigger `lcm` to change value to false throughout the entire `range` array,
  - move to the next `if` statement. If `lcm` is still `true`, return `num * m` to terminate the `while` loop.

- This is a `brute force` approach to find the smallest common multiple.
- Even I could see that it is inefficient.
- If I pass large numbers, even though the code still passes, the console throws a 'potential infinite loop' warning.

## Drop It

- Given the array arr, iterate through and remove each element starting from the first element (the 0 index) until the function func returns true when the iterated element is passed through it.
- Then return the rest of the array once the condition is satisfied, otherwise, arr should be returned as an empty array.

- The challenge sounded simple enough. But I was wrong.

```js
function dropElements(arr, func) {
  let newArr= [];
  for (let i = 0; i < arr.length; i++) {
    if (func(arr[i])) {
      newArr.push(arr[i]);
    }
  }
  return newArr;
}
```

- This solution passes most of the test cases.
- It's fine as long as the numbers are in numerical order.
- The iteration should stop when it finds the first element that passes `true` for the function passed to the function `dropElements`

- So I tried this

```js
function dropElements(arr, func) {
  for (let i = 0; i < arr.length; i++) {
    if (func(arr[i])) {
      arr = arr.slice(i);
      break;
    }
  }
  return arr;
}
```

- The above code passes as long as the elements in the array are within the range specified in the function passed as the argument.
- but if `dropElements([1, 2, 3, 4], function(n) {return n > 5]})` is passed to the function, since the array elements doesn't reach the condition, it just returns the original array.
- Then I remembered it should return an empty array if the condition is not satisfied.

```js
function dropElements(arr, func) {
  let newArr = [];
  for (let i = 0; i < arr.length; i++) {
    if (func(arr[i])) {
      newArr = arr.slice(i);
      break;
    }
  }
  return newArr;
}
```

- The above solution passes the challenge, but there is one thing that is bugging me.
- `dropElements([1, 2, 3], function(n) {return n < 3; })` should return [1, 2]. But it returns [1, 2, 3]
- The problem is that the `for` loop is setup to break when the function condition is satisfied.
- The code passes as long as n is greater than or equal to a value.
- As soon as you encounter 'less than or equal to', the first element in the array will satisfy the function and break the `for` loop.
- In this case, arr[0], which is 1, satisfies n < 3.
- It breaks the `for` loop and returns the sliced `arr` from index 0 to the end of the array.
- I could use an infinite `while` loop like I did in the previous challenge, but I think that is defeating the purpose of the challenge.
- I need to find out another way to go about this. Something other than a `for` loop.

- I was fooling myself this whole time.
- I was too fixated on `n < 3` that I thought the return value should be [1, 2], which is not the case.
- The challenge was to return the rest of the array as soon as the condition is satisfied.
- 1 < 3 is true so it is correct to return [1, 2, 3]

## Steamroller

- Flatten a nested array. You must account for varying levels of nesting.

- Had to do some googling since I had no idea what to do here.
- Some new methods were introduced.
  - `Array.isArray()`
    - This checks if an object passed is an array.
    - It returns `true` if an object is an array, and `false` otherwise.
  - `.some()`
    - This method tests whether an element passes a test implemented by the provided function.
    - It returns `true` if the test passes, and `false` otherwise.
- These methods can be combined to create a condition for a `while` loop to run until the condition returns `false`

```js
function steamrollArray(arr) {
  while(arr.some((item) => Array.isArray(item))) {
    arr = [].concat(...arr);
  }
  return arr;
}
```

- function `steamrollArray` receives an array `arr` as its argument.
- `arr` is multidimensional and its depth is unknown.
  - There is a `.flat()` method that can be used, but not for the purpose of this challenge

```js
function steamrollArray(arr) {
  arr = arr.flat(Infinity);
  return arr;
}
```

- Use a `while` loop to run until the specified condition returns false.
  - The condition will check if the item passed to the callback function is an array.
- If it is an array, the spread operator is used to take that element and concatenate it to an empty array.
- This process repeats recursively and continues to check for nested arrays in the original array.
- If there are no more nested arrays in the original array, the callback function returns false and the loop terminates.

## Binary Agents

- Return an English translated sentence of the passed binary string.
- The binary string will be space separated.

- It would be easier if the string was an array of individual strings for each binary code.
- Use `split` method to do just that.

```js
function binaryAgents(str) {
  str = str.split(' ')
}
```

- The `HINT` section suggested that it would be easier to convert the binary to decimal values.
  - "Converting binary to decimal first simplifies the translation process by allowing us to use a lookup table to find the corresponding ASCII character for each group of 8 binary digits, rather than trying to manually translate each individual binary digit into an ASCII character."
- I had forgotten about the `parseInt()` function, which takes a string argument and returns an integer of the specified `radix`.
- Use `.map()` to iterate through the array created using split and use the `parseInt` function with `radix = 2` to convert binary to decimal.

```js
function binaryAgents(str) {
  str = str.split(' ')
           .map(elem => parseInt(elem, 2))
}
```

- Then use `.map()` once again with the convenient `String.fromCharCode()` to return a string created from the array of decimal values.

```js
function binaryAgents(str) {
  str = str.split(' ')
           .map(elem => parseInt(elem, 2))
           .map(elem => String.fromCharCode(elem))
}
```

- Now all of the binary values have been converted to its corresponding characters.
- Use `.join()` method to put the sentence together.

```js
function binaryAgents(str) {
  str = str.split(' ')
           .map(elem => parseInt(elem, 2))
           .map(elem => String.fromCharCode(elem))
           .join('');
  return str;
}
```
