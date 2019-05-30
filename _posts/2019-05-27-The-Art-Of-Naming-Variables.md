---
layout: post
title: The Art Of Naming Variables
tags: [Programming Standard]
---

Once I were rich, I would absolutely hire a secretary helping me name functions and variables.

# Introduction

As a non-native speaker in English, I almost got frustrated every time I try to name varibles. Therefore, I've found some awesome posts about variables naming, which are listed below.

# Naming different data structures

> Src: https://hackernoon.com/the-art-of-naming-variables-52f44de00aad

###Arrays

Arrays are an iterable list of items, usually of the same type. Since they will hold multiple values, **pluralizing** the variable name makes sense.

```javascript
// bad
const fruit = ['apple', 'banana', 'cucumber'];
// okay
const fruitArr = ['apple', 'banana', 'cucumber'];
// good
const fruits = ['apple', 'banana', 'cucumber'];
// great - "names" implies strings
const fruitNames = ['apple', 'banana', 'cucumber'];
// great
const fruits = [{
    name: 'apple',
    genus: 'malus'
}, {
    name: 'banana',
    genus: 'musa'
}, {
    name: 'cucumber',
    genus: 'cucumis'
}];
```

### Booleans

Booleans can hold only 2 values, `true` or `false`. Given this, using prefixes like “**is**”, “**has**”, and “**can**” will help the reader infer the type of the variable.

```javascript
// bad
const open = true;
const write = true;
const fruit = true;
// good
const isOpen = true;
const canWrite = true;
const hasFruit = true;
```

What about predicate functions (functions that return booleans)? It can become tricky to name the value, after naming the function.

```javascript
const user = {
    fruits: ['apple']
}
const hasFruit = (user, fruitName) => (
    user.fruits.includes(fruitName)
);
// what do we name this boolean?
const x = hasFruit(user, 'apple');
```

We can’t name the boolean `hasProjectPermission`, as we’ve already given that name to the function. In this case, I like to prefix my predicates with either `check` or `get`.

```javascript
const checkHasFruit = (user, fruitName) => (
    user.fruits.includes(fruitName)
);
const hasFruit = checkHasFruit(user, 'apple');
```

### Numbers

For numbers, think about words that describe numbers. Words like `maximum`, `minimum`, `total` will .

```javascript
// bad
const pugs = 3;
// good
const minPugs = 1;
const maxPugs = 5;
const totalPugs = 3;
```

### Functions

Functions should be named using a verb, and a noun. When functions perform some type of action on a resource, its name should reflect that. A good format to follow is `actionResource`. For example, `getUser`.

```javascript
// bad
userData(userId)
userDataFunc(userId)
totalOfItems(items)
// good
getUser(userId);
calculateTotal(items);
```

A common convention I’ve seen used for transforming values is prefixing function names with `to`.

```javascript
// I like it!
toDollars('euros', 20);
toUppercase('a string');
```

Another common naming pattern I like is when iterating over items. When receiving the argument inside the function, use the singular version of the array name.

```javascript
// bad
const newFruits = fruits.map(x => {
    return doSomething(x);
});
// good
const newFruits = fruits.map(fruit => {
    return doSomething(fruit);
});
```

# Naming your variables!

> src: https://blog.usejournal.com/naming-your-variables-f9477ba002e9

### Names should be pronounceable

Humans are good at words. Anything well pronounceable is easy to catch.

Let, we have a function named `getDMY()` which returns day-month-year of a schedule. Suppose, you need to describe your code to someone. Now, what will be the pronunciation of `getDMY` ? Confused, YES?
Renaming it to `getDayMonthYear()` can solve your problem instantly!

# Compound nouns

### Forming the plural of compound nouns

The plurals of compound nouns are generally formed by **adding 's' to the *principal* word** (i.e. the most significant word in the compound), also called the ***head*** of the compound. The ***head*** informs us about what the compound refers to. The other elements of the compound modify the ***head*** and are called the **head's dependents**.

Examples:

|     compounds     |    head    |       plural       |
| :---------------: | :--------: | :----------------: |
|   car **park**    |  **park**  |   car **parks**    |
|  black**board**   | **board**  |  black**boards**   |
| **mother**-in-law | **mother** | **mothers**-in-law |
|  taxi **driver**  | **driver** |  taxi **drivers**  |

# Should directories/routers/filenames be named in singular or plural

Use plurals if there's even the possibility of there being multiple items in that directory/router/filename (whether now, in the past, or in the future). If on your desk you had a box labelled "Pencils" which contained five pencils, and you took four out, you wouldn't suddenly decide that the "Pencils" label was incorrect and needed to be replaced with one that just said "Pencil". You'd just think that you were running out of pencils.

On the other hand, if there will never be more than one item in that directory/router/filename, no matter what happens in the future, then it would probably be more intention-revealing to use a singular name.

