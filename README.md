![Aftermath](https://github.com/hyperoslo/Aftermath/blob/master/Images/cover.png)

[![CI Status](http://img.shields.io/travis/hyperoslo/Aftermath.svg?style=flat)](https://travis-ci.org/hyperoslo/Aftermath)
[![Version](https://img.shields.io/cocoapods/v/Aftermath.svg?style=flat)](http://cocoadocs.org/docsets/Aftermath)
[![Carthage Compatible](https://img.shields.io/badge/Carthage-compatible-4BC51D.svg?style=flat)](https://github.com/Carthage/Carthage)
![Swift](https://img.shields.io/badge/%20in-swift%202.2-orange.svg)
[![License](https://img.shields.io/cocoapods/l/Aftermath.svg?style=flat)](http://cocoadocs.org/docsets/Aftermath)
[![Platform](https://img.shields.io/cocoapods/p/Aftermath.svg?style=flat)](http://cocoadocs.org/docsets/Aftermath)

## Description

**Aftermath** is a stateless message-driven micro-framework in Swift, which is
based on the concept of the unidirectional data flow architecture.

At first sight **Aftermath** may seem to be just a type-safe implementation of
the publish-subscribe messaging pattern, but actually it could be considered as
a distinct mental model in application design, different from familiar MVC,
MVVM or MVP approaches. Utilizing the ideas behind
[Event Sourcing](http://martinfowler.com/eaaDev/EventSourcing.html)
and [Flux](https://facebook.github.io/flux/) patterns it helps to separate
concerns, reduce code dependencies and make data flow more predictable.

## Core components

The following diagram demonstrates a simplified version of the flow
in **Aftermath** architecture and defines 4 main components that form the core
of the framework:

<div align="center">
<img src="https://github.com/hyperoslo/Aftermath/blob/master/Images/diagram1.png" />
</div><br/>

### Command

**Command** is a message with a set of instructions describing an intention to
execute the corresponding behavior. Command could lead to data fetching,
data mutation and any sort of sync or async operation that
produces desirable output needed to update application/view state.

Every command can produce only one output type.

### Command Handler

**Command Handler** layer is responsible for business logic in the application.
The submission of a command is received by a command handler, which usually
performs short- or long-term operation, such as network request, database query,
cache read/white process, etc. Command handler can be sync and publish the
result immediately. On the other hand it's the best place in the
application to write asynchronous code.

The restriction is to create only one command handler for each command.

### Event

**Command Handler** is responsible for publishing **events** that will be
consumed by reactions. There are 3 types of events:

- **Progress** event indicates that the operation triggered by command has been
started and is in the pending state at the moment.
- **Data** event holds the output produced by the command execution
- **Error** notifies that an error has been occurred during the command
execution

### Reaction

**Reaction** responds to event published by command handler. It is supposed
to handle 3 possible event types by describing the desired behavior in the
each scenario:

- **Wait** function reacts on `Progress` type of the event
- **Consume** function reacts on `Data` type of the event.
- **Rescue** function is a fallback for the case when `Error` type of the event
has been received.

Normally reaction performs UI updates, but could also be used for other kinds
of output processing.

## Installation

**Aftermath** is available through [CocoaPods](http://cocoapods.org). To install
it, simply add the following line to your Podfile:

```ruby
pod 'Aftermath'
```

**Aftermath** is also available through [Carthage](https://github.com/Carthage/Carthage).
To install just write into your Cartfile:

```ruby
github "hyperoslo/Aftermath"
```

**Aftermath** can also be installed manually. Just download and drop `Sources`
folders in your project.

## Extensions

This repository aims to be the core implementation of framework, but there are
also a range of extensions that integrate **Aftermath** with other libraries
and extend it with more features:

- [AftermathTools](https://github.com/hyperoslo/AftermathTools) is a set of
development tools for **Aftermath** where you can find additional helpers,
useful command and event middleware for logging, error handling, etc.

- [AftermathCompass](https://github.com/hyperoslo/AftermathCompass) is a
message-driven routing system built on top of **Aftermath** and
[Compass](https://github.com/hyperoslo/Compass).

- [AftermathSpots](https://github.com/hyperoslo/AftermathSpots) is made to
improve development routines of building component-based UIs using
[Spots](https://github.com/hyperoslo/Spots) cross-platform view controller
framework. It comes with custom reactions and injectable behaviors that move
code reusability to the next level and make your application even more
decoupled and flexible.

## Examples

- [iOS Playground](https://github.com/hyperoslo/Aftermath/blob/master/Playground-iOS.playground/Content.swift)
uses live view of interactive playground to show how to fetch data from network
and display it in the `UITableView`.

- [AftermathNotes](https://github.com/hyperoslo/Aftermath/blob/master/Example/AftermathNotes)
is a simple application that demonstrates how to setup networking stack and
data cache layer using **Aftermath**. It uses the concept of `stories` to group
related types and make the `command -> event` flow more readable.

- [AftermathNotesPlus](https://github.com/hyperoslo/Aftermath/blob/master/Example/AftermathNotes)
is a more advanced example that extends [AftermathNotes](https://github.com/hyperoslo/Aftermath/blob/master/Example/AftermathNotes)
demo. It plays with generics and introduces the concept of `features` in order
to reuse view controllers and RESTful network requests.

## Author

Hyper Interaktiv AS, ios@hyper.no

## Influences

**Aftermath** is inspired by the idea of unidirectional data flow in
[Flux](https://facebook.github.io/flux/) and utilizes some concepts like
sequence of commands and events from
[Event Sourcing](http://martinfowler.com/eaaDev/EventSourcing.html).

## Contributing

We would love you to contribute to **Aftermath**, check the [CONTRIBUTING](https://github.com/hyperoslo/Aftermath/blob/master/CONTRIBUTING.md)
file for more info.

## License

**Aftermath** is available under the MIT license. See the [LICENSE](https://github.com/hyperoslo/Aftermath/blob/master/LICENSE.md)
file for more info.
