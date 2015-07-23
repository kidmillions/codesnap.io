---
title: Rust and FFI
tags: rust, Javascript, node.js
---

In the little time I've had between mad coding sessions at Hack Reactor, where we are in the midst of thesis projects, I've tried to pick up [Rust](https://doc.rust-lang.org) as a second language, often with competing frustration/excitement.

My learning goal in the coming months is to engage with a low-level language. While Javascript has the community and the [hope for world dominance](http://radar.oreilly.com/2013/04/will-javascript-take-over-the-programming-world.html), its runtime limitations are very, very real. It will continue to be the ~~only~~ usual way for client-side web programming for the foreseeable future. I wonder, however, even as Node has brought Javascript into a sort of server-side ubiquity, whether I should try something a more performant and with less abstraction when working in the backend.

Among Rust's advantages are its use of the stack over the heap, which rules out [garbage collection](https://en.wikipedia.org/wiki/Garbage_collection_(computer_science)), as well Rust's ability to force concurrency (or *threading*) when making huge computations. [Here's a bit by the Rust people on their stack implementation](https://doc.rust-lang.org/stable/book/the-stack-and-the-heap.html).

Of course, the disadvantage is that Rust can be a a real bummer to write correctly, like any low-level language that extensively uses standard libraries and static typing. Javascript is just **soooo goood** at keeping your programs simple, readable, and compatible from platform to platform, despite its many flaws.

There is a middle way, as I have recently discovered. Rust provides a robust **Foreign Function Interface (FFI)** that can both call into lower level languages, as well as provide itself to higher level languages. I hope to use Javascript whenever performance is not an issue, but find appropriate times to call on Rust *within* my Javascript programs.

### Easy Steps to Expose Your Rust to Your Javascript Program and Change Your Life Forever

Expose your .rs file and make sure the compiled function names are the same as in your precompiled code:
```
#[no_mangle] //This ensures function names are not mangled in compiled rust
pub extern fn main() {
  //The pub extern type exposes this function to be callable from outside.
}
```

Change your crate.toml file to expose a dynamic library:
```
crate-type = ["dylib"] //instead of 'rlib', which is Rust-specific
```

Upon building with crate, check for the .so file. There's the moneymaker: your fully exposed 'shared object' for use by your JS program.

Install the ['ffi' npm module](https://www.npmjs.com/package/ffi) and 'import' the shared object into your program as if it were a module:

```
var ffi = require('ffi');

var lib = ffi.Library('path/to/so/file', {
  'main': [ 'void', []  ]
});

lib.main();

```

Note that `'void'` is the return type for the shared object function, and the array is for arguments passed into the function.

I'm blown away by the simplicity of it all, and I plan to incorporate Rust into some of the high-maintenance aspects of my thesis project.

Now all I have to do is get a working, professional knowledge of Rust. Wish me luck.
