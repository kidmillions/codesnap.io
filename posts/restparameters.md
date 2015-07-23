---
title: ES6 and You (the New-ish Engineer) Rest and Default Parameters
tags: Javascript, es6
---

Be merry, and rejoice! The ECMAScript 2015 spec has [passed muster](http://www.infoq.com/news/2015/06/ecmascript-2015-es6), and Javascript is well on its way to ~annual iteration and improvement. In the spirit of the season, here's another great aspect of ES6 that pumps me up.

Before talking about rest parameters, though, I also want to signal boost the [traceur grunt task](https://github.com/aaronfrost/grunt-traceur) to compile your ES6 code properly while we meanwhile wait for browsers to fully integrate everything.

### No More Slicing and Dicing Arguments! Use Rest Parameters Instead

CoffeeScript pros are probably well-aware of rest parameters (also known as [**splats**](http://coffeescript.org/#splats)), and this feature is coming to ES6. Say we wanted to populate a list of all the things we brought to lunch today. It could be one thing, or a thousand, and for that reason, we'd use the Arguments object.

```
var getLunch = function() {
  var result = "<ul><li>";
  var args = Array.prototype.slice.call(arguments);
  result += args.join("</li><li>");
  result += "</li></ul>"; // end list
  return result;
};

getLunch('starfish', 'coffee', 'maple syrup', 'jam');

```

The Array-like object [Argument](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/arguments?redirectlocale=en-US&redirectslug=JavaScript%2FReference%2FFunctions_and_function_scope%2Farguments) is our best bet, but it doesn't have a native `.join()` method. We have to use the Array prototype's `.slice()` to make an shallow array copy. ES6's rest parameter simplifies things significantly defining a 'collection of arguments' with `...`:

```
var getLunch = function(...food) {
  var result = "<ul><li>";
  result += food.join("</li><li>");
  result += "</li></ul>"
  return result;
};

getLunch('starfish', 'coffee', 'maple syrup', 'jam');
```

This yields the same result as the other example, without explicit handling/conversion of the Arguments object.

We can put a rest parameter as the last parameter in a list, as well:

```
var checkOut(first, second, ...rest) {
  console.log(first);
  console.log(second);
  console.log(rest); //will log array of arguments passed after first and second
};

```

No longer do we need to access `arguments.length` to splice any remaining arguments out. They are right there for the taking.

### Default Parameters Are Your Friend

Default parameters are self-explanatory:
```
var myName = function(name='Prince') {
  return 'My name is ' + name + '.';
};

myName('Chris'); //returns 'My name is Chris.'
myName(''); //returns 'My name is .'
myName(); //returns 'My name is Prince.'
myName(undefined) //returns 'My name is Prince.'
```

This, of course, can also be done with multiple parameters within a single function definition.

### Things to Watch Out For

* **A parameter cannot be both a default parameter and a rest parameter.** Although you can use both rest and default parameters in the same definition, something like `function myName(...name='Chris')` is going to throw an error.
* **Don't use functions as default parameters.** Although variables can be passed in as default parameters, functions are not going to evaluate inline for you, even if you try to invoke them through mind control.

Here's to forever avoiding that rather pesky Arguments object. Please share your thoughts below, and let me know how you are using ES6 features in your day-to-day code.
