# Frequency Counter

This pattern uses objects or sets to collect values/frequency of values   
This can often avoid the need for nested loops or O(N^2) operations with array / string

## An example

### Problem
Write a function called **same**, which accepts two arrays. The function should return true if every value in the array has it's corresponding value
squared in the second array. The frequency of values must be same.
```js
same([1, 2, 3], [4, 1, 9]); // true
same([1, 2, 3], [1, 9]); // false
same([1, 2, 1], [4, 4, 1]); // false (must be the same frequency)
```

### the Naive Solution
```js
function same(arr1, arr2) {
  if(arr1.length !== arr2.length) {
    return false;
  }
  for (let i = 0; i < arr1.length; i++) {
    const correctIndex = arr2.indexOf(arr[i] ** 2);
    if(correctIndex === -1) {
      return false;
    }
    arr2.splice(correctIndex, 1);
  }
  return true;
}
```

### Refactor 
```js
function same(arr1, arr2) {
  if(arr1.length !== arr2.length) {
    return false;
  }
  let frequencyCounter1 = {};
  let frequencyCounter2 = {};
  for(let val of arr1) {
    frequencyCounter1[val] = (frequencyCounter1[val] || 0) + 1;
  }
  for(let val of arr2) {
    frequencyCounter2[val] = (frequencyCounter2[val] || 0) + 1;
  }
  for(let key in frequencyCounter1) {
    if(!(key ** 2 in frequencyCounter2)) {
      return false;
    }
    if(frequencyCounter2[key ** 2] !== frequencyCounter1[key]){
      return false
    }
  }
  return true;
}
```

## Anagrams
 Given two strings, write a function to determine if the second string is an anagram of the first.   
 An anagram is a word, phrase, or name formed by rearranging the letters of another, such as cinema, formed from iceman.
 
### My Solution
```js
function validAnagram(str1, str2){
  if(str1.length !== str2.length) {
      return false;
  }
  let frequencyCounter1 = {};
  let frequencyCounter2 = {};
  for(const el of str1) {
      (el in frequencyCounter1) ? frequencyCounter1[el]++ : frequencyCounter1[el] = 1;
  }
  for(const el of str2) {
      (el in frequencyCounter2) ? frequencyCounter2[el]++ : frequencyCounter2[el] = 1;
  }
  for(let val in frequencyCounter1) {
      if(!(val in frequencyCounter2)){
          return false;
      }
      if(frequencyCounter1[val] !== frequencyCounter2[val]){
          return false;
      }
  }
  return true;
}
```

### Reference Solution

```js

function validAnagram(str1, str2){
  if(str1.length !== str2.length) {
      return false;
  }
  
  let lookup = {};
  for(const el of str1) {
    lookup[el] ? lookup[el]++ : lookup[el] = 1;
  }
  for(let i = 0; i < str2.length; i++) {
    const letter = str2[i];
    if(!lookup[letter]) {
      return false;
    } else {
      lookup[letter]--;
    }
  }
  return true;
}
```
