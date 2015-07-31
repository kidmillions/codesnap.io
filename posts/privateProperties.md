---
title: ES6 and You! Private Properties
tags: es6, Javascript
published: true
---

I can't help but feel like I'm behind. As I slowly accumulate better understanding of ES6 concepts, others are already [writing and talking about ES7](https://www.youtube.com/watch?v=DqMFX91ToLw). Regardless, I'm come with more goodies coming to the language to share this week!

Before I get into magical private properties and symbols, I encourage readers to look at [Babel's awesome mini-tutorial](https://babeljs.io/docs/learn-es2015/), which includes its own REPL!

### Symbols and Private Properties

ES6 is introducing **symbols**, which are essentially new primitive types that can do remarkable things, one of which is to provide easy notation for private properties on classes.

Symbols can do this easily because they can create value without any risk of naming collisions with other parts of the module / program.

```
const album = Symbol(); // scoped to function or module
```

Let's also define a `class`. [Remember our new class syntax?](http://blog.chrisclayman.com/harmony-and-the-intermediate-developer/)

```
class Musician{
  constructor(name, instrument, albumName) {
    this.name = name;
    this.instrument = instrument;
  }
}
```

Shhh! we want to keep each musician's upcoming album name private. We also do not want to give others any opportunity to edit the album name value. Let's use the symbol.

```
const album = Symbol();

class Musician{
  constructor(name, instrument, albumName) {
    this.name = name;
    this.instrument = instrument;
    this[album] = albumName;
  }
}


// now make a musician
var prince = new Musician('Prince', 'everything', 'Love Symbol');

```

We are actually using the constant symbol as the key to the property `albumName`.

Until said key is made public, without a symbol type, there's no way at getting at the album names.

```
console.log(prince); // logs {name: 'Prince', instrument: 'everything'}

for(var key in prince){
  console.log("For-in loop over obj: " + key + ":" + beast[key]);
    //logs through name and instrument
    // no trace of album name
}
```

For programs, and games in particular (where score is really important and super unchangeable), symbols are going to be a true way forward.

Thoughts? Please get in touch with me via comments!
