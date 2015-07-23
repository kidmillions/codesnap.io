---
title: ES6 and You (the New-ish Engineer) Superclasses and Subclasses
---

By now, most everyone who works with Javascript knows that there are some **truly wacko** upcoming changes to ECMAScript with the approval of the ES6 spec. That wacko-ness is most apparent to those of us who have been set in our Javascriptisms for our entire programming life. We drop our jaws at the sight of the [latest spec draft](http://wiki.ecmascript.org/doku.php?id=harmony:specification_drafts). But all the hullabaloo fails to see that Javascript is **finally** (albeit slowly) becoming a better programming language. Developers would be remiss not to take advantage of this.

There are some really great and brief [cheatsheets](http://espadrine.github.io/New-In-A-Spec/es6/) and [videos](https://www.youtube.com/watch?v=rwm5JLqCpdk) scattered around the web, but it can be hard to parse what's most important to the average dev. I particularly enjoyed Sasha Bayan's talk at Hack Reactor last week. He has set up a [great walkthrough](https://github.com/SashaBayan/ES6-Starter-Kit) of the most important additions to ES6.

Over the next few posts, I'll outline a few things that are particularly exciting, especially as they apply to concepts and practices used in the early weeks of [Hack Reactor](http://www.hackreactor.com).

###TADA! Subclass Inheritance is Now Extension

Currently, the common practice for building subclasses in pseudoclassical style goes like:

```
//Pre-ES6
var Rain = function(wetness) {
  this.wetness = wetness;
};

// First define the subclass constructor.
var PurpleRain = function(wetness) {
  Rain.call(this, wetness); // Call the superclass constructor on subclass
  this.color = 'purple';
}
// Overwrite and extend the prototype.
PurpleRain.prototype = Object.create(Rain.prototype);
// Reassign the correct constructor.
PurpleRain.prototype.constructor = PurpleRain;
```

This is an unholy mess, no surprise there. Notice that in order to designate `Rain` as a superclass, we had to first delegate the `PurpleRain` prototype to that of its superclass, and then overwrite the constructor to its original value. Let's take a look at how it works with ES6-style class extension.

```
//ES6
class Rain {
  constructor(wetness){
    this.wetness = wetness;
  }
}

class PurpleRain extends Rain {
  constructor(wetness) {
    super(wetness); //invokes superclass constructor
    this.color = 'purple';
  }
}
```

Beautiful, with lovely echoes of [Java](http://leftoversalad.tumblr.com/post/103503118002) classes. We simply had to invoke the superclass constructor within the subclass, and use the `extends` keyword to delegate to the correct prototype.

####But Wait...There's More

Methods defined on the prototype can be messy. Why not just place them in the class definitions?

In some cases, I've seen devs put methods within the contructor function, but that's a [big no-no when you are instantiating many objects](http://stackoverflow.com/questions/9772307/declaring-javascript-object-method-in-constructor-function-vs-in-prototype).

Because we explicitly define the `constructor` in our new ES6 class definitions, we can declare shared methods immediately after.

```
//Pre-ES6

function Rain(wetness) {
  this.wetness = wetness;
}

Rain.prototype.makeItRain = function() {
  return this.wetness;
}

//ES6
class Rain {
  constructor(wetness) {
    this.wetness = wetness;
  }
  makeItRain() {
    return this.wetness;
  }
}
```

I find this to be a much cleaner and more modular way of representing classes.  External prototypal changes, like what we see often with classes, seems to me to be a sort of necessary evil.

####But Wait, There's More More

Subclasses can easily access their superclass's methods. No need to use .apply() or .call(), just the keyword `super`:

```
//ES6
class PurpleRain extends Rain {
  constructor(wetness) {
    super(wetness); //invokes super-class constructor
    this.color = 'purple';
  }
  makeItRain() {
    return this.color + super.makeItRain();
  }
}
```

The `super` keyword is accessible by the subclass,  and allows us to easily extend methods, which can lead to all sorts of fun.

Got any other great uses for ES6 classes? Definitely share, or [shoot me a tweet](http://www.twitter.com/chrisclayman).


