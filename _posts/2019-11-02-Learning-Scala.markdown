---
layout: post
comments: true
title: Syntax sugar and anonymous functions in Scala
mathjax: false
excerpt: "Learning Scala is a pertinent process, and I think building good knowledge about Syntax
sugars and anonymous functions and know how these two things actually expand to what is really
important"
---

# Common expansions of syntax sugars and anonymous functions

### Constructors and Functions as a first class citizen
We know scala is a functional programming language, and it is designed by mathematician. According to wiki, *Apply* is applying a function to its arguments.
In Java, you would commonly  expect:
```java
Tree t = new Tree(args)
```
But in Scala, a `Tree(args)` is actually `Tree.apply(args)` so `apply` method in Scala is the method you should override when you want to provide your functions as an object


```scala
object Tree {
  def apply[A](aseq: A*): Tree[A] = {
    ... // saved for brevity
  }
}
```
* [reference on stackoverflow](https://stackoverflow.com/questions/9737352/what-is-the-apply-function-in-scala)
* Quote from the above link:
> `apply` serves the purpose of closing the gap between Object-Oriented and Functional paradigms in Scala

### How to use underscores

```scala
// ERROR: missing parameter type for expanded function
myStrings.foreach(println(_.toString))
```

it expands to:
```scala
myStrings.foreach(println(_ => _.toString))
```
it annoys the compiler because compiler does not know the type of `_`


* [reference on stackoverflow](https://stackoverflow.com/questions/7627117/scala-underscore-error-missing-parameter-type-for-expanded-function)
* Quote from it:
> The placeholder syntax for anonymous functions replaces `the smallest` possible containing expression with a function.


### Pass Seq or List to vararg functions

There is a very example of when we need the varargs instead of Seq. A container is holding a type `A`, and its members are expecting type A as well:
```scala
class ContainerClass[+A] {
  def apply(aSeq: A*): ContainerClass[A] = ...
}
```
If we pass the seq to the `apply` function, it will actually infer the A to be the type of `Seq[A]`, so we need the `A`, not the `Seq[A]`. We can use this call:
```scala
ContainerClass.apply(aseq: _*)
```
The compiler will pass `A` instead of `Seq[A]` to the function

* [reference stackoverflow](https://stackoverflow.com/questions/1832061/scala-pass-seq-to-var-args-functions)

### Good online Scala material

* [Scala exercise](https://www.scala-exercises.org/fp_in_scala/functional_data_structures)
* [For the book fpinscala, amazingly it has a scala exercise](https://www.scala-exercises.org/fp_in_scala/functional_data_structures)

### Pattern matching

In Scala, pattern matching is a function too. The compiler will create anonymous function for us. The compiler needs to be able to tell the type of the arguments in the function.

* [Good stackoverflow read](https://stackoverflow.com/questions/12869251/the-argument-types-of-an-anonymous-function-must-be-fully-known-sls-8-5)

The nice quotes:
```scala
{ case X(x) => ... } is a partial function, but the compiler still doesn't know what your input type is, except that it's a supertype of X. Normally this isn't a problem because if you're writing an anonymous function, the type is known from the context.
```

And the error we should learn from:
```Scala
{case QualifiedType(preds, ty) =>
               t.ty = ty ;
               Some((emptyEqualityConstraintSet,preds)) }
```
