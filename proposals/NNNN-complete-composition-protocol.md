# Complete Swift Composition Model using Protocol

* Proposal: [SE-NNNN](NNNN-complete-composition-protocol.md)
* Author(s): [Wallacy Freitas](https://github.com/Wallacy)
* Status: **(TBD)**
* Review manager: **(TBD)**

## Introduction

One weakness of single inheritance is inability to split or model across more than one dimension. Understanding how make a good design using Swift Language can save countless hours of development.

Today, Swift only supports single inheritance, however Swift Protocols can be used to get pretty close of the mantra: [Composition over inheritance](http://en.wikipedia.org/wiki/Composition_over_inheritance).

## Motivation

Without enter too deep into Composition pattern, here is a quick dummy example:

```swift
class Robot{
    func drive() { }
}
class MurderRobot: Robot{
    func kill() { }
}
class Animal{
    func poop() { }
}
class Dog: Animal {
    func bark() { }
}
class Cat: Animal {
    func meow() { }
}
class Bird: Animal {
    func fly() {}
}
```
Let's pretend you are developing a game, which have Animal, Robots and other characters... You game is running fine, couple of months of development go by, and the project manager ask you to implement: A Flying-Killer-Dog-Robot!!

Using single inheritance model probably you will need make a lot of changes in the model, and depending how you will solve the problem, will need rewrite some functions like "bark" and "fly".

This is not unsolved problem using inheritance, that's not the point here.

> But the really big problem with inheritance is that you’re encouraged to predict the future. Inheritance encourages you to build this taxonomy of objects very early on in your project, and you are most likely going to make design mistakes doing that, because humans cannot predict the future (even though it feels like we can), and getting out of these inheritiance taxonomies is a lot harder than getting out of them. [*][1]

Today, this exactly same problem can be solved in Swift using the "Protocol First" approach, this mean, instead define a hierarchical model contemplating objects, you should instead, declare protocols with respective actions for this objects.

>Inheritance is when you design your types around what they are, and composition is when you design types around what they do. [*][1]

```swift
protocol Automatable{
    func drive()
}
extension Automatable {
    func drive() { }
}
protocol Killer {
    func kill()
}
extension Killer {
    func kill() { }
}
protocol Pooper{
    func poop()
}
extension Pooper{
    func poop(){ }
}
protocol Barker{
    func bark()
}
extension Barker{
    func bark(){ }
}
protocol Meower{
    func meow()
}
extension Meower{
    func meow(){ }
}
protocol Flying{
    func fly()
}
extension Flying{
    func fly(){ }
}

struct Bird: Pooper,Flying {
}
struct Dog: Pooper, Barker {
}
struct MurderRobot: Automatable, Killer {
}
struct Cat: Pooper, Meower {
}
struct FlyingKillerDogRobot: Automatable, Barker, Killer, Flying {
}
```

The good part is, you can compose your types as you wish, without restrictions. Also, you are not restrict to reference types, like a class, can choose structs if want.


## Proposed solution

Describe your solution to the problem. Provide examples and describe
how they work. Show how your solution is better than current
workarounds: is it cleaner, safer, or more efficient?

## Detailed design

Describe the design of the solution in detail. If it involves new
syntax in the language, show the additions and changes to the Swift
grammar. If it's a new API, show the full API and its documentation
comments detailing what it does. The detail in this section should be
sufficient for someone who is *not* one of the authors to be able to
reasonably implement the feature.

## Impact on existing code

Describe the impact that this change will have on existing code. Will some
Swift applications stop compiling due to this change? Will applications still
compile but produce different behavior than they used to? Is it
possible to migrate existing Swift code to use a new feature or API
automatically?

-------------------------------------------------------------------------------

[1]: https://medium.com/humans-create-software/composition-over-inheritance-cb6f88070205
