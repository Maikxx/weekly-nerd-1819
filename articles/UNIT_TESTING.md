# Unit testing

This is one of three articles regarding the experiences that I gathered in the work field and during the minor web development. Some things are new to me, for example testing applications, which is exactly what this article is about.

This article focusses on how unit testing disproves your thoughts and implementations of the easiest appearing pieces of code in an application. I have not a lot of experience with this, so if you find something that is not completely right or could be better, feel free to shoot an issue in the repository.

## Table of Contents

* [Introduction](#Introduction)
* [Getting started](#Getting-started)
* [Example](#Example)
* [Conclusion](#Conclusion)
* [Resources](#Resources)

## Introduction

> "In computer programming, unit testing is a software testing method by which individual units of source code, sets of one or more computer program modules together with associated control data, usage procedures, and operating procedures, are tested to determine whether they are fit for use." - Dorota Huizinga

Okay, sounds nice, but what does it actually mean in English?
Well, if you have read my [other article about TypeScript](./TYPESCRIPT.md), you know that it is very time efficient to have errors thrown at you at compile time, so you don't have to debug things run-time.

While this is true for TypeScript libraries, JavaScript libraries are compiled at run-time, so they won't give you any indication that there is something wrong with your code, unless you write tests for them, which you can run before starting the application.

The ideal combination to remove human errors from your code is by using TypeScript and write unit tests.

I am by no means an expert in unit testing, but I already came across a very useful use case in my [latest project](https://github.com/Maikxx/360-wallscope). I use TypeScript on the Node server, with a PostgreSQL database. Again, I am by no means a data architect or PostgreSQL professional, so I could use every hand I could get.

As a challenge for myself during this _meesterproef_ I hit up the challenge to improve performance of the application. While you could just simply compress all outgoing files, I also went a step further and wanted to take a look at how a very performant database such as PostgreSQL could best be written.

In this article I will explore some of the principles and code of unit testing, while also showing you the case where I thought I had everything under control in a simple function, which appeared to break the whole application after testing it.

## Getting started

When I started to take a look at unit testing I knew a few things for sure already, for example, did you know there are a ton of JavaScript unit testing frameworks?
To name a few: Jasmine, Mocha, AVA, Tape and Jest.

I did some research, mainly DuckDuckGo-ing "Unit testing TypeScript applications". After doing this and reading [this article on unit testing frameworks](https://raygun.com/blog/javascript-unit-testing-frameworks/) I decided to go for Mocha.
Mocha was pretty much every single search the top most used one, so I went with that.

If you want to get to know more about how Mocha works from the inside, please [refer to the documentation](https://mochajs.org/).

Mocha in itself does nothing, it needs an _assertion library_ to go with it and run the tests. Just like with the testing frameworks itself, there are [a few assertion libraries out there](https://js.libhunt.com/libs/assertion).

Following the logic behind my choice for Mocha, I also decided to choose the one that came up in the searches most often, which was [Chai.js](https://www.chaijs.com/). It is by no means the most popular in terms of stars on GitHub, but it does provide a clean way to interact with it.

As of the moment of writing the most used testing framework for JavaScript does not have TypeScript support like Mocha does, which is also a huge perk for this package.

Before you can add a test to your application, make sure that you have a _package.json_ file.

So to put it all together and start testing your TypeScript application, you would run the following command: `yarn add chai mocha ts-node @types/chai @types/mocha -D` or `npm install chai mocha ts-node @types/chai @types/mocha -D`.

This in itself is not enough to run tests, you will need to specify a test in the _package.json_ file with the register it needs to use to precompile code (in the case of TypeScript), so that it can be tested with Mocha, as well as an entry point for your test files.

This entry point can be a single file, or a glob pattern.

For example, to test every file ending with the name _.test.ts_ in the `server/src` folder, you will need to do something like this:
```json
{
    "scripts": {
        "test": "mocha -r ts-node/register server/src/**/*.test.ts"
    }
}
```

While unit testing you come across one of two types of file name conventions: `.spec.ts` or `.test.ts` as extensions. I couldn't find a good resource which explains why to use one or the other, so I tried to take a look at why something would be called _spec_, which is when I came to [this website](https://en.wikipedia.org/wiki/Specification_(technical_standard)), which explains it pretty well.

As far as my understanding goes there is no physical benefit to use one over the other, so I decided to go with the `.test.ts` extension, since it is a unit _testing_ file.

The example we are going to be focussing on next is a _Node_ application. When you want to run tests for client applications, there are some additional steps to be taken, which [this article](https://journal.artfuldev.com/unit-testing-node-applications-with-typescript-using-mocha-and-chai-384ef05f32b2) explains very well.

## Example

As mentioned previously, I recently came across a very good use case for unit testing in my own applications.
Before that I thougth it was quite excessive and time consuming to write tests for all functionalities of your application, which I still sort of think is not always necessary, but oh well.

Let's assume you have the following piece of code in a lot of places throughout your application: \``{${array.join(', ')}}`\`, this piece turns an array into a PostgreSQL array input type. The more you know...

So when I saw this code was being used in a lot of places I decided to take matters in my own hands and create a function out of it, which can be reused.

Like this (in `converters.ts`):

```typescript
export function convertToPostgreSQLArray(array: any[]): string {
    return `{${array.join(', ')}`
}
```

At this point I didn't bother testing it, because well... it's only one line of code. So I replaced all the instances where I used the initial way to convert it (the repeated array.join) with this function and passed the array to it.

A day passed and I started to take on unit testing and thought, well gotta start small, right?
Good thing I did, otherwise I would have probably never noticed the error. Can you see it?

<details>
<summary>Spoilers</summary>

There is a curly brace missing at the end of the string.
</details>

So here is the test file I created (`converters.test.ts`):

```typescript
import { convertToPostgreSQLArray } from './converters'
import { expect } from 'chai'
import 'mocha'

describe('ConvertToPostgreSQLArray', () => {
    it('Should convert a regular one-dimensional array of items into a PostgreSQL array format', () => {
        const oneDimension = convertToPostgreSQLArray([ 1, 2, 3 ])
        expect(oneDimension).to.equal(`{1, 2, 3}`)

        const multiDimensional = convertToPostgreSQLArray([ 1, 2, 3, [ 1, 2, 3 ]])
        expect(multiDimensional).to.equal(`{1, 2, 3, 1,2,3}`)
    })
})
```

When I ran the first test I only had the first _expect_, which for some reason failed! Taking a closer look at it and I found that I had broken all database queries involving this function after the refactor due to a missing curly brace.

Amazing! There is the benefit of testing presented to you. When refactoring code it is often easy to break some things in the application relying on that piece of code. Unit testing helps you (besides, you know, just taking better care when refactoring) avoid these issues.

Also notice the second _expect_, and the description of the test, which says it needs to convert a single-dimensional array to PostgreSQL format. I for one did not know that when you had an `array.join(', ')`, you would also flatten the additional arrays for the case when it had these. This meant that it does not join these inner arrays according to the rule you set (`', '`), but instead just stringifies it without a space, which failed the test.

This is just a simple example of how to use unit testing, it can and will be way more complex as you go on, but the principle remains the same.

## Conclusion

When it comes to unit testing, as you may have seen it can really help you out in some situations where you are just in a hurry and trying to do a lot of things at once, causing bugs.
With unit testing you can iron those bugs out before they even occur in production.
There is a lot left to be learned and explored about unit testing, as it is a whole job category on it's own, so make sure to check out these resources below.

## Resources

* [Automated Defect Prevention: Best Practices in Software Management](https://www.wiley.com/en-us/Automated+Defect+Prevention%3A+Best+Practices+in+Software+Management-p-9780470042120)
* [Chai.js](https://www.chaijs.com/)
* [JavaScript unit testing frameworks: Comparing Jasmine, Mocha, AVA, Tape and Jest \[2018\]](https://raygun.com/blog/javascript-unit-testing-frameworks/)
* [Mocha.js](https://mochajs.org/)
* [Unit testing a TypeScript library](https://www.tsmean.com/articles/how-to-write-a-typescript-library/unit-testing/)
* [Unit testing node applications with TypeScript using Mocha and Chai](https://journal.artfuldev.com/unit-testing-node-applications-with-typescript-using-mocha-and-chai-384ef05f32b2)