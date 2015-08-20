---
title: ES6 and You (the New-ish Engineer) It's a Modules World, We're Just Living In It
tags: Javascript, es6
---

Give the ECMA committee props—they did a great job of listening to the community by simply opening their eyes to the best practices and newest tools in Javascript World. I've written before about how Coffeescript syntax/concepts are [creeping into some features](http://blog.chrisclayman.com/es6-and-you-the-new-ish-engineer-rest-and-default-parameters/). Modules, which (through CommonJs and AMD standards) changed the way many developers structure their code, **have their own implementation in ES6**. If you are new-ish to modules, [here is a good technical introduction](http://addyosmani.com/writing-modular-js/), and [the argument for modularity](http://eloquentjavascript.net/1st_edition/chapter9.html), a tool found in almost every language besides Javascript.

The hardest part for those who are used to CommonJS / RequireJS / WhateverJS is to determine where ES6 modules should fit into the ecosystem. The [ECMA people](http://www.ecma-international.org/memento/TC39.htm) intend for ES6 modules to be a sort of compromise between the standards. So, where will this feature find some elbow room?

### The power of ES6 Modules

Let's put together a nice module file:

```
// greetings.js

export function hello(name = "world") {
  console.log("Hello, ${name}!");
}

export function goodnight(name = "world") {
  console.log("Goodnight, sweet ${name}!");
}

```

And now I can use this in a new file:

```
// main.js

import { hello, goodnight } from "./greetings.js"

//I can now invoke both functions
hello("Prince"); //logs "Hello, Prince!"
goodnight("Prince"); //logs "Goodnight, sweet Prince!"

```

Modules can be aliased quite easily, assuming you have defined functions in a file to import:

```
// greetings.js
function goodnight() {
  console.log("goodnight, moon.");
}

export {goodnight as soLong}

// main.js
import {soLong as peaceOut} from "./greetings.js"
peaceOut(); // logs "Goodnight, moon."
```

As you can see, I've aliased the function goodnight twice: once upon export from greetings.js, and again during import.

You can, obviously, also just export an entire library object with `export * as (ALIAS) from (MODULE PATH)`, which would allow for syntax you are likely used to seeing through CommonJS modules.

So, you have great flexibility during your exports. You can explicitly export a value with the `export` keyword, *or* you can export the entire file as an accessible object.



### Ok, So Why Not Just Continue With CJS and/or AMD?

Because CJS and AMD were created **precisely to address the lack of modules in Javascript–features that are now arriving natively.** CommonJS has been great for server-side code, and AMD works better in browsers, but their syntax never played nice outside of their home environments–the module.exports object in CJS was difficult in client-side code, and AMD's `define()` seems restrictive to some.

ES6 modules, with their flexibility and with the promise of [module loaders](https://people.mozilla.org/~jorendorff/js-loaders/Loader.html), should close the gaps that have been bridged adequately (but not perfectly) by CJS and AMD standards, and should give the Javascript community one unified standard for modularity. Because, as you know, the Javascript community never fragments and picks sides. ;)

Any thoughts about modules in ES6? Please share below.
