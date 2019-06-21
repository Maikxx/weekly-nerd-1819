# How to use TypeScript to write less buggy code

This is one of three articles regarding the experiences that I gathered in the work field and during the minor web development. Some things are new to me, for example testing applications, while others, like this one I exclusively use myself and have learned at [Lifely](https://lifely.nl/).

This article focusses on the way [TypeScript](https://www.typescriptlang.org/docs/home.html) can help you to write less buggy code. You will have to test the product less, spend less time in the `debugger` and get rid of that absurd amount of `console.log()` statements.

## Table of Contents

* [Introduction](#Introduction)
* [Installation](#Installation)
* [TypeScript Types](#Typescript-types)
* [But why?](#But-why?)
* [Conclusion](#Conclusion)
* [Resources](#Resources)

## Introduction

> TypeScript is an open-source programming language developed and maintained by [Microsoft](https://www.microsoft.com/en-us/).

TypeScript is a superset on top of another commonly used programming language: JavaScript.
TypeScript is, in contrast to JavaScript, a _loosely typed_ language. This means that you will need to define all types of the values of variables and return types of functions.
TypeScript has a feel when writing it that is similar to both JavaScript and [C#](https://docs.microsoft.com/en-us/dotnet/csharp/).

TypeScript can be customized in many ways to make it as _loosely typed_ as JavaScript, or as _strongly typed_ as Java, C or C#.

Generally, languages like Java require you to always type your variables. Where as in TypeScript you have the ability to let variables be implicitly typed by the compiler. This sounds like JavaScript, doesn't it? Well yes, but no. In TypeScript implicitly typed variables can still give you errors if you were to dynamically change this variable to something of another type, with the exception of a variable being of `any` type.

## Installation

To start using TypeScript in your projects you will need to have TypeScript installed on your computer or have the TypeScript Visual Studio Code extension.

Installing TypeScript globally on your computer is as easy as running the following command on your computer:
`yarn global add typescript` or `npm install typescript -g`.

The nice thing with working with TypeScript, is that a lot of frameworks and libraries have TypeScript types, as you can see [here](https://www.typescriptlang.org/samples/index.html) and [here](https://definitelytyped.org/).

This means that there is barely anything you need to do extra to start using in existing JavaScript projects.
You just need to add a [tsconfig.json](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html) file and change some development scripts to start using it.

You can change a lot of things in the tsconfig.json file, [here](https://www.typescriptlang.org/docs/handbook/compiler-options.html) you can find all the options for this file. There is a lot of these options that I have personally never ever even used or fiddled with.

I most of the time use a boilerplate that I configured a tiny bit from the one they use at [Lifely](https://lifely.nl). You can take a look at the config file we use for the _meesterproef_ of the minor web development , at the AUAS [here](../assets/tsconfig.json). This is a configuration file for client-side code compiling. To use this in Node, you will need to tweak some things.

## TypeScript Types

In TypeScript all values can be seen as being of a certain _type_.
In TypeScript you `type` a variable so that it's value that it contains must be of this type.
This means that they fall into one of many categories, below are some of the most widely used.
You can find all the types and how to use them [here, for basic types](https://www.typescriptlang.org/docs/handbook/basic-types.html) and [here, for advanced types, which can use an article on their own](https://www.typescriptlang.org/docs/handbook/advanced-types.html).

### Booleans

Example: `const isHavingFun: boolean = true`

### Numbers

In JavaScript and TypeScript **all** numbers are _floats_. This is the first major difference with most other _strongly typed_ languages, where numbers each fall into their own category, for example: `integer`, `float` and much more.

You could argue saying the TypeScript way is easier to remember, while also being more error prone.

Example: `const myNumber: number = 42`

### Strings

Example: `const myString: string = 'Hello world!'`

### Arrays

If an array only consists of one certain type of value, for example an array of numbers, then you can do something like the following:

* `const myLittleArray: number[] = [4, 2, 42]`
* `const myLittleArray: Array<number> = [4, 2, 42]`

I personally use the first of these two examples, but you could opt to take the former. The former makes use of the _generic array type_.

## But why?

You may think something along the lines of "Okay cool, what gives?". While I had that mindset originally as well before using it, I quickly realized this language is so much more powerful than JavaScript itself.

Most people don't see the payoff for setting up a project in TypeScript (to be honest, it's only like 5 minutes of work either way).

When working on a project in JavaScript, how often do you have to check in the `console` in the browser if something is passed to, for example, a function correctly, if that parameter exists and if it is an object, what properties are definitely on there? Probably a lot of times, right?

TypeScript is so useful, since it simply won't compile if you have made a mistake regarding a type and similar issues. In the command line you will see an error message, which you can then track to debug the problem in your code, before even looking at the browser window (in the case of client-side applications).

This doesn't only save you a lot of time, but also a lot of headaches and frustration, which is what I get when I write JavaScript nowadays.

While I am a firm pro-TypeScript user, I can see for some applications where it might not be necessary, let's say you only have about 50 lines of code, you will probably be done debugging that error way earlier than when you had setup TypeScript and started to write code then.

For cases, which are well beyond the scope of 50 lines of code ([this is a nice example](https://github.com/Maikxx/360-wallscope), as well as [this](https://github.com/Maikxx/jiskefet) if you want to take a look. Shameless plugs), you will definitely want to use TypeScript.

I'll show you a little example which probably would cause you to start writing console logs everywhere, were you not to use TypeScript. It might be a stupid example, but oh well.

### JavaScript

```javascript

const dataObject = {
    name: 'John Doe',
    age: 42,
}

function getNameAndAge(user) {
    return `My name is ${user.name} and I am ${user.age} years old.`
}

console.log(getNameAndAge(dataObject))

```

### TypeScript

```typescript

interface User {
    name?: string
    age?: number
}

const dataObject: User = {
    name: 'John Doe',
    age: 42,
}

function getNameAndAge(user: User) {
    if (user.name && !isNaN(Number(user.age))) {
        return `My name is ${user.name} and I am ${user.age} years old.`
    }

    throw new Error(`You have not passed the name or age properties on the user object correctly.`)
}

console.log(getNameAndAge(dataObject))

```

Thanks to the smart syntax highlighting you know when you need to add extra checks in your code for cases where the code otherwise might not work. If you now were to forget to pass user to the function, the code will not run. Also if you get data from a database, like a user, they don't always have the properties on the object that you require. This makes it difficult to trust your own code, which is why _typing these variables_ is a good idea if you want to make sure something works 99% of the time.

## Conclusion

I like TypeScript, everyone knows that who knows me.
While I think it is fun and easy to use, it _can_ be overwhelming to set up, especially in large scale projects.
You should definitely have a firm grasp of JavaScript itself before also learning to use TypeScript.

When you think you are ready to start learning it, [start here](https://www.typescriptlang.org/) and work your way through the documentation. You will not regret it, it will save you a lot of time in the long run!

## Resources

* [TypeScript in 5 minutes](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html)
* [TypeScript official website](https://www.typescriptlang.org/)
* [TypeScript Types](https://www.typescriptlang.org/docs/handbook/basic-types.html)
* [Strongly typed language explanation](https://way2java.com/java-introduction/strongly-typed-language/)