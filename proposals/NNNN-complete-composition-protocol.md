# Complete Swift Composition Model using Protocol

* Proposal: [SE-NNNN](NNNN-complete-composition-protocol.md)
* Author(s): [Wallacy Freitas](https://github.com/Wallacy)
* Status: **(TBD)**
* Review manager: **(TBD)**

## Introduction

One weakness of single inheritance is inability to split or model across more than one dimension. Understanding how make a good design using composition with Swift Language can save countless hours of development.

Today, Swift only supports single inheritance, however Swift Protocols can be used to get pretty close of the mantra: [Composition over inheritance](http://en.wikipedia.org/wiki/Composition_over_inheritance).

## Motivation

Without enter too deep into Composition pattern, here is a quick dummy example:

```swift
class Robot{
    func drive() {
      //..//
    }
}
class MurderRobot: Robot{
    func kill() {
      //..//
    }
}
class Animal{
    func poop() {
      //..//
    }
}
class Dog: Animal {
    func bark() {
      //..//
    }
}
class Cat: Animal {
    func meow() {
      //..//
    }
}
class Bird: Animal {
    func fly() {
      //..//
    }
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
    func drive() {
      //..//
    }
}
protocol Killer {
    func kill()
}
extension Killer {
    func kill(){
      //..//
    }
}
protocol Pooper{
    func poop()
}
extension Pooper{
    func poop(){
      //..//
    }
}
protocol Barker{
    func bark()
}
extension Barker{
    func bark() {
      //..//
    }
}
protocol Meower{
    func meow()
}
extension Meower{
    func meow(){
      //..//
    }
}
protocol Flying{
    func fly()
}
extension Flying{
    func fly() {
      //..//
    }
}
```

```swift
struct Bird: Pooper,Flying {}
struct Dog: Pooper, Barker {}
struct MurderRobot: Automatable, Killer {}
struct Cat: Pooper, Meower {}
struct FlyingKillerDogRobot: Automatable, Barker, Killer, Flying {}
```

The good part is, you can compose your types as you wish, without restrictions. Also, you are not restrict to reference types, you can choose value types if want.

However, the first downside is the typing repetition. Each function is declared on the protocol body and on protocol extension. Also, typing repetition get worse when you handle with properties.

```swift
protocol Flying{
    var numberOfWings:Int { get set }
    var wingArea:Int { get set }
    var beatsPerSecond:Int { get set }
    func fly()
}
extension Flying{
    func fly() {
      // can using numberOfWings
      // wingArea and beatsPerSecond
    }
}
protocol Pooper{
    func poop()
}
extension Pooper{
    func poop(){
      //..//
    }
}
struct SomeBird: Pooper,Flying {
    var numberOfWings = 2
    var wingArea = 100
    var beatsPerSecond = 10
}
```
Leaving aside repetitions, all this powerful cannot be used to fully implement the "Composition Model". Protocols cannot be used to implement abstract classes for example. Also, protocols cannot be used to implement object hierarchy! We can not keep private implementations on protocol, default implementations cannot be overridden and call the default again. These and other minor limitations preclude the use of so-called Protocol Oriented Programing in the real world programing.

And more, besides the handful ability to use  Self or associated type requirements direct on protocols, this leave us a lot of limitations:

```swift
protocol GenericProtocol {
	typealias AbstractType
	func magic() -> AbstractType
}
let list : [GenericProtocol] = []
// Error: Protocol 'GenericProtocol' can only be used as a generic constraint because it has Self or associated type requirements.
```

This is a simple example of the different capabilities of Swift protocols, this is very nice feature, also, confusing for some people.

The idea of this proposal is to discuss small steps necessary to make protocols completely usable to produce compositions (for classes and structures) in a simplest way.

Each step also, can be a separated proposal if needed.

## Proposed solution

### Call protocol default implementation directly.

static dispath, guarantie not call children

### Eliminate need for extensions to the default implementation.

### Implicit declaration of properties using default values.

### Required Initializers on Protocols

trick, partial initilization,

### Freely declare super types (value and reference types)

### Use super to call first declared protocol implementation.


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
