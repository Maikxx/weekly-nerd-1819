# How to use TypeScript to write less buggy code

This is one of three articles regarding the experiences that I gathered in the work field and during the minor web development. Some things are new to me, for example testing applications, while others, like this one I exclusively use myself and have learned at [Lifely](https://lifely.nl/).

This article focusses on the way [TypeScript](https://www.typescriptlang.org/docs/home.html) can help you to write less buggy code. You will have to test the product less, spend less time in the `debugger` and get rid of that absurd amount of `console.log()` statements.

## Table of Contents

* [Introduction](#Introduction)
* [Installation](#Installation)
* [TypeScript Types](#Typescript-types)
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

## How it may help you

## Resources

* [TypeScript in 5 minutes](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html)
* [TypeScript official website](https://www.typescriptlang.org/)
* [TypeScript Types](https://www.typescriptlang.org/docs/handbook/basic-types.html)
* [Stronly typed language explanation](https://way2java.com/java-introduction/strongly-typed-language/)