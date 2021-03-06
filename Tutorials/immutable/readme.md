# Anjana Vakil: Learning Functional Programming with JavaScript - JSUnconf 2016

https://www.youtube.com/watch?v=e-5obm1G_FY

## immutable.js

> https://facebook.github.io/immutable-js/  
> 


## mori

> http://swannodette.github.io/mori/  
> https://www.npmjs.com/package/mori  
> https://github.com/ufo-github/mori/tree/master/Tutorails

A library for using ClojureScript's persistent data structures and supporting API from the comfort of vanilla JavaScript.

Clojure === java ?

ClojureScript === js compiler




****************************************************************************************************************
## Getting started

> Install immutable using npm.
```sh
$ npm install immutable
``` 
Then require it into any module.
```sh
var Immutable = require('immutable');
var map1 = Immutable.Map({a:1, b:2, c:3});
var map2 = map1.set('b', 50);
map1.get('b'); // 2
map2.get('b'); // 50
``` 
Browser

To use immutable from a browser, download dist/immutable.min.js or use a CDN such as CDNJS or jsDelivr.

Then, add it as a script tag to your page:

```sh
<script src="immutable.min.js"></script>
<script>
    var map1 = Immutable.Map({a:1, b:2, c:3});
    var map2 = map1.set('b', 50);
    map1.get('b'); // 2
    map2.get('b'); // 50
</script>
``` 
Or use an AMD loader (such as RequireJS):
```sh
require(['./immutable.min.js'], function (Immutable) {
    var map1 = Immutable.Map({a:1, b:2, c:3});
    var map2 = map1.set('b', 50);
    map1.get('b'); // 2
    map2.get('b'); // 50
});
``` 
If you're using browserify, the immutable npm module also works from the browser.

TypeScript

Use these Immutable collections and sequences as you would use native collections in your TypeScript programs while still taking advantage of type generics, error detection, and auto-complete in your IDE.

Just add a reference with a relative path to the type declarations at the top of your file.
```sh
///<reference path='./node_modules/immutable/dist/immutable.d.ts'/>
import Immutable = require('immutable');
var map1: Immutable.Map<string, number>;
map1 = Immutable.Map({a:1, b:2, c:3});
var map2 = map1.set('b', 50);
map1.get('b'); // 2
map2.get('b'); // 50
``` 
The case for Immutability

Much of what makes application development difficult is tracking mutation and maintaining state.   
Developing with immutable data encourages you to think differently about how data flows through your application.

Subscribing to data events throughout your application creates a huge overhead of book-keeping which can hurt performance, sometimes dramatically, and creates opportunities for areas of your application to get out of sync with each other due to easy to make programmer error.   
Since immutable data never changes, subscribing to changes throughout the model is a dead-end and new data can only ever be passed from above.

This model of data flow aligns well with the architecture of React and especially well with an application designed using the ideas of Flux.

When data is passed from above rather than being subscribed to, and you're only interested in doing work when something has changed, you can use equality.

Immutable collections should be treated as values rather than objects. While objects represents some thing which could change over time, a value represents the state of that thing at a particular instance of time. This principle is most important to understanding the appropriate use of immutable data. In order to treat Immutable.js collections as values, it's important to use the Immutable.is() function or .equals() method to determine value equality instead of the === operator which determines object reference identity.
```sh
var map1 = Immutable.Map({a:1, b:2, c:3});
var map2 = map1.set('b', 2);
assert(map1.equals(map2) === true);
var map3 = map1.set('b', 50);
assert(map1.equals(map3) === false);
``` 
Note: As a performance optimization Immutable attempts to return the existing collection when an operation would result in an identical collection, allowing for using === reference equality to determine if something definitely has not changed. This can be extremely useful when used within memoization function which would prefer to re-run the function if a deeper equality check could potentially be more costly. The === equality check is also used internally by Immutable.is and .equals() as a performance optimization.

If an object is immutable, it can be "copied" simply by making another reference to it instead of copying the entire object.   
Because a reference is much smaller than the object itself, this results in memory savings and a potential boost in execution speed for programs which rely on copies (such as an undo-stack).
```sh
var map1 = Immutable.Map({a:1, b:2, c:3});
var clone = map1;
``` 
JavaScript-first API

While immutable is inspired by Clojure, Scala, Haskell and other functional programming environments, it's designed to bring these powerful concepts to JavaScript, and therefore has an Object-Oriented API that closely mirrors that of ES6 Array, Map, and Set.

The difference for the immutable collections is that methods which would mutate the collection, like push, set, unshift or splice instead return a new immutable collection.   
Methods which return new arrays like slice or concat instead return new immutable collections.
```sh
var list1 = Immutable.List.of(1, 2);
var list2 = list1.push(3, 4, 5);
var list3 = list2.unshift(0);
var list4 = list1.concat(list2, list3);
assert(list1.size === 2);
assert(list2.size === 5);
assert(list3.size === 6);
assert(list4.size === 13);
assert(list4.get(0) === 1);
``` 
Almost all of the methods on Array will be found in similar form on Immutable.List, those of Map found on Immutable.Map, and those of Set found on Immutable.Set, including collection operations like forEach() and map().
```sh
var alpha = Immutable.Map({a:1, b:2, c:3, d:4});
alpha.map((v, k) => k.toUpperCase()).join();
// 'A,B,C,D'
``` 
Accepts raw JavaScript objects.

Designed to inter-operate with your existing JavaScript, immutable accepts plain JavaScript Arrays and Objects anywhere a method expects an Iterable with no performance penalty.
```sh
var map1 = Immutable.Map({a:1, b:2, c:3, d:4});
var map2 = Immutable.Map({c:10, a:20, t:30});
var obj = {d:100, o:200, g:300};
var map3 = map1.merge(map2, obj);
// Map { a: 20, b: 2, c: 10, d: 100, t: 30, o: 200, g: 300 }
``` 
This is possible because immutable can treat any JavaScript Array or Object as an Iterable. You can take advantage of this in order to get sophisticated collection methods on JavaScript Objects, which otherwise have a very sparse native API.   
Because Seq evaluates lazily and does not cache intermediate results, these operations can be extremely efficient.
```sh
var myObject = {a:1,b:2,c:3};
Immutable.Seq(myObject).map(x => x * x).toObject();
// { a: 1, b: 4, c: 9 }
``` 
Keep in mind, when using JS objects to construct Immutable Maps, that JavaScript Object properties are always strings, even if written in a quote-less shorthand, while Immutable Maps accept keys of any type.
```sh
var obj = { 1: "one" };
Object.keys(obj); // [ "1" ]
obj["1"]; // "one"
obj[1];   // "one"

var map = Immutable.fromJS(obj);
map.get("1"); // "one"
map.get(1);   // undefined
```
Property access for JavaScript Objects first converts the key to a string, but since Immutable Map keys can be of any type the argument to get() is not altered.

Converts back to raw JavaScript objects.

All immutable Iterables can be converted to plain JavaScript Arrays and Objects shallowly with toArray() and toObject() or deeply with toJS().   
All Immutable Iterables also implement toJSON() allowing them to be passed to JSON.stringify directly.
```sh
var deep = Immutable.Map({ a: 1, b: 2, c: Immutable.List.of(3, 4, 5) });
deep.toObject() // { a: 1, b: 2, c: List [ 3, 4, 5 ] }
deep.toArray() // [ 1, 2, List [ 3, 4, 5 ] ]
deep.toJS() // { a: 1, b: 2, c: [ 3, 4, 5 ] }
JSON.stringify(deep) // '{"a":1,"b":2,"c":[3,4,5]}'

``` 
Embraces ES6

Immutable takes advantage of features added to JavaScript in ES6, the latest standard version of ECMAScript (JavaScript), including Iterators, Arrow Functions, Classes, and Modules.  
It's also inspired by the Map and Set collections added to ES6. The library is "transpiled" to ES3 in order to support all modern browsers.

All examples are presented in ES6. To run in all browsers, they need to be translated to ES3.
```sh
// ES6
foo.map(x => x * x);
// ES3
foo.map(function (x) { return x * x; });

``` 
Nested Structures

The collections in immutable are intended to be nested, allowing for deep trees of data, similar to JSON.
```sh
var nested = Immutable.fromJS({a:{b:{c:[3,4,5]}}});
// Map { a: Map { b: Map { c: List [ 3, 4, 5 ] } } }
``` 

A few power-tools allow for reading and operating on nested data.   
The most useful are mergeDeep, getIn, setIn, and updateIn, found on List, Map and OrderedMap.
```sh
var nested2 = nested.mergeDeep({a:{b:{d:6}}});
// Map { a: Map { b: Map { c: List [ 3, 4, 5 ], d: 6 } } }
nested2.getIn(['a', 'b', 'd']); // 6

var nested3 = nested2.updateIn(['a', 'b', 'd'], value => value + 1);
// Map { a: Map { b: Map { c: List [ 3, 4, 5 ], d: 7 } } }

var nested4 = nested3.updateIn(['a', 'b', 'c'], list => list.push(6));
// Map { a: Map { b: Map { c: List [ 3, 4, 5, 6 ], d: 7 } } }
``` 
Lazy Seq

Seq describes a lazy operation, allowing them to efficiently chain use of all the Iterable methods (such as map and filter).

Seq is immutable — Once a Seq is created, it cannot be changed, appended to, rearranged or otherwise modified.  

Instead, any mutative method called on a Seq will return a new Seq.

Seq is lazy — Seq does as little work as necessary to respond to any method call.

For example, the following does not perform any work, because the resulting Seq is never used:
```sh
var oddSquares = Immutable.Seq.of(1,2,3,4,5,6,7,8)
  .filter(x => x % 2).map(x => x * x);
``` 
Once the Seq is used, it performs only the work necessary.   
In this example, no intermediate arrays are ever created, filter is called three times, and map is only called once:
```sh
console.log(oddSquares.get(1)); // 9
``` 
Any collection can be converted to a lazy Seq with .toSeq().
```sh
var seq = Immutable.Map({a:1, b:1, c:1}).toSeq();
``` 
Seq allow for the efficient chaining of sequence operations, especially when converting to a different concrete type (such as to a JS object):
```sh
seq.flip().map(key => key.toUpperCase()).flip().toObject();
// { A: 1, B: 1, C: 1 }

``` 
As well as expressing logic that would otherwise seem memory-limited:
```sh
Immutable.Range(1, Infinity)
  .skip(1000)
  .map(n => -n)
  .filter(n => n % 2 === 0)
  .take(2)
  .reduce((r, n) => r * n, 1);
// 1006008
``` 
Note: An iterable is always iterated in the same order, however that order may not always be well defined, as is the case for the Map.

Equality treats Collections as Data

Immutable provides equality which treats immutable data structures as pure data, performing a deep equality check if necessary.
```sh
var map1 = Immutable.Map({a:1, b:1, c:1});
var map2 = Immutable.Map({a:1, b:1, c:1});
assert(map1 !== map2); // two different instances
assert(Immutable.is(map1, map2)); // have equivalent values
assert(map1.equals(map2)); // alternatively use the equals method
``` 
Immutable.is() uses the same measure of equality as Object.is including if both are immutable and all keys and values are equal using the same measure of equality.

Batching Mutations

If a tree falls in the woods, does it make a sound?

If a pure function mutates some local data in order to produce an immutable return value, is that ok?

— Rich Hickey, Clojure  
Applying a mutation to create a new immutable object results in some overhead, which can add up to a minor performance penalty.  
If you need to apply a series of mutations locally before returning, Immutable gives you the ability to create a temporary mutable (transient) copy of a collection and apply a batch of mutations in a performant manner by using withMutations.   
In fact, this is exactly how Immutable applies complex mutations itself.

As an example, building list2 results in the creation of 1, not 3, new immutable Lists.
```sh
var list1 = Immutable.List.of(1,2,3);
var list2 = list1.withMutations(function (list) {
  list.push(4).push(5).push(6);
});
assert(list1.size === 3);
assert(list2.size === 6);
``` 
Note:   
immutable also provides asMutable and asImmutable, but only encourages their use when withMutations will not suffice.   
Use caution to not return a mutable copy, which could result in undesired behavior.

Important!:   
Only a select few methods can be used in withMutations including set, push and pop.   
These methods can be applied directly against a persistent data-structure where other methods like map, filter, sort,   
and splice will always return new immutable data-structures and never mutate a mutable collection.


## Documentation

Read the docs and eat your vegetables.

Docs are automatically generated from Immutable.d.ts. Please contribute!

Also, don't miss the Wiki which contains articles on specific topics. Can't find something? Open an issue.

## Testing

If you are using the Chai Assertion Library, Chai Immutable provides a set of assertions to use against Immutable collections.

## Contribution

Use Github issues for requests.

We actively welcome pull requests, learn how to contribute.

## Changelog

Changes are tracked as Github releases.

## Thanks

Phil Bagwell, for his inspiration and research in persistent data structures.

Hugh Jackson, for providing the npm package name. If you're looking for his unsupported package, see this repository.

## License

Immutable is BSD-licensed. We also provide an additional patent grant.









