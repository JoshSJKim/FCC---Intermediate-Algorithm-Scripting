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