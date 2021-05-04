---
layout: post
title: Word chaning
subtitle: Practice with backward recursive and pointer in C.
cover-img: /assets/img/path.jpg
thumbnail-img: /assets/img/thumbnail/1.jpg
share-img: /assets/img/path.jpg
gh-repo: https://github.com/hadomanh/water-jug-puzzle
gh-badge: [star, fork, follow]
tags: [C, algorithms, recursive, problem]
---

### Problem:
> Given a list of newline-separated handles create an arrangement such that the following word begins with the same letter the prior word ended with.

### Display:

| Input | Output |
| - | - |
|4 <br> seek red karaoke dads | red dads seek karaoke|
|2 <br> stressed desserts| desserts stressed |

### Solution:
- Use backward recursive function
    - Return **true** if array of string can be arranged as word chain
    - Else return **false**
- Anchor case: Return **true** when input has only 1 character
- Divide array of string into 2 parts:
    - Part 1: A word in array. Put this word to end of array.
    - Part 2: Remaining words. 
    - If part 2 can be sorted into word chain and part 1 can join at the end of part 2, return **true**
    - If the above condition are not met, move last word back into original position and try another word

### Pseudo code:
```
bool sort(array of n words)

    If n = 1 
        Return true

    Loop through n words
        Swap current word with the last word in array

        Call sort(the first n-1 word in array)

        If recursive function return true && current word can join with sorted array
            Return true

        Put current word to original position and try another word

    Return false
```

### Example:
Take a look at this example:
```sh
Input: dads seek red
```
Call ```sort([dads, seek, red])```:
- Loop 1:
    - Swap: [red, seek, ***dads***]
    - Call ```sort([red, seek])```:
        - ```[seek, red]```: false
        - ```[red, seek]```: false
    - Back: [ dads, seek, red ]

- Loop 2:
    - Swap: [ dads, red, ***seek***]
    - Call ```sort([dads, red])```:
        - ```[red, dads]```: true
    - Join ```[red, dads, seek]```: true
    
Return true

```sh
Output: red, dads, seek
```


{: .box-note}
**Note:** Source code available [here](https://github.com/hadomanh/code-challenge/tree/master/Word%20Chaning). Give me ⭐ if you enjoy it!