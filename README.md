# Swift — Optionals

## Objectives

## Introduction

*I call it my billion-dollar mistake. It was the invention of the null reference in 1965. At that time, I was designing the first comprehensive type system for references in an object oriented language (ALGOL W). My goal was to ensure that all use of references should be absolutely safe, with checking performed automatically by the compiler. But I couldn't resist the temptation to put in a null reference, simply because it was so easy to implement. This has led to innumerable errors, vulnerabilities, and system crashes, which have probably caused a billion dollars of pain and damage in the last forty years.*  
—Tony Hoare, apologizing for including the null reference in ALGOL W

// improve reference description and link

#### `nil`: Something From Nothing

There's an old riddle that goes, "what can you put in a barrel to make it lighter?" While there's a [variety of answers](https://answers.yahoo.com/question/index?qid=20080303110423AA6pGbZ) that could apply, the classic answer is "a hole." The adage perfectly encapsulates the irony of conceptualizing nothingness as *some*thing (being a hole).

How can nothingness be something? Perhaps only as a result of human abstraction. However, the *idea* of nothingness is actually something—it's an idea! In a rather metaphysical twist absence becomes presence.

![](shadowlink)  
—*Link and Shadow Link*, Zelda fan art by [Qacachis](http://www.desktopwallpapers4.me/games/link-vs-dark-link-7966/)

Like Shadow Link in *The Legend of Zelda*, many programming languages include a representation of nothingness — that ancient, sleeping evil with names known to all: `nil`, `null`, `Nil`, `NULL`, `NSNull`, [Gothmog](http://lotr.wikia.com/wiki/Gothmog), and [Darkside](https://youtu.be/i9Ab5z0d8OI).

But if `nil` is so evil, then why was it included in any programming language to begin with? That's been [a topic of much debate](http://programmers.stackexchange.com/a/237699) in programming circles for decades; but for as much as it causes problems, it does have its uses.

// explain main use cases: internet responses, accessing a collection (53rd card from a deck)

#### `nil` in Swift

The Swift programming language is a "[strongly typed](https://en.wikipedia.org/wiki/Strong_and_weak_typing)" language. This means that the compiler enforces the object types. Since `nil` is its own special type, the Swift compiler will not allow it to be assigned to a variable of any class.

```
class Present {
    var description: String = nil     // error
}
```

So, how can we assign the absence of a value to a variable that *cannot* be nothing? Enter **Optionals**.

?? http://stackoverflow.com/questions/24043589/null-nil-in-swift-language ??

## Optionals

Swift handles `nil` compatibility by automatically generating what is called the "optional-type" of every possible class. The language handles this distinction with the syntax of appending a `?` ("question mark") to the end of the class name (though you may also see `Optional<T>` or `Optional<Element>` floating around the internet). So, if we have a class `SomeClass`, its optional type will be `SomeClass?`.

This "optional" types provides a "wrapper" for the class that can encapsulate `nil`. It essentially reads as *an-instance-of-SomeClass-or-nil*, as opposed to the normal *definitely-an-instance-of-SomeClass*. It's important to understand the distinction here; **a class's type and its optional type are not the same type.** An instance of the `SomeClass?` optional cannot be assigned to a variable of the `SomeClass` class without first being "unwrapped" from the potential that it actually contains `nil`.

The optional is defining the expectation that an object will be of a specific class but without committing to it actually having any contents. It's like having presents under a Christmas tree before Christmas Day: there's every expectation that underneath the colorful box-shaped wrapping paper there resides a plethora of highly-anticipated swag. However, without being able to open the presents until the given time, we can't be *certain* that the paper actually contains anything other than thin air. For all we explicitly know until Christmas morning, each one of the presents could actually be a cruel trick.

The decisions to make Swift a strongly-typed language with the inclusion of optionals is in part to reduce the problems caused by `nil`. By requiring that the potential-to-be-`nil` is explicitly stated, the Swift compiler the eliminates "shadowy places" in which `nil` can hide and end up causing unexpected behavior:

```
class Present {
    var description = "firetruck"
}

class ChristmasTree {
    var presents = [Present?]()
    
    func openNextPresent() -> Present {
        let present = self.presents.popLast()  // error
        return present                         // error
    }
}
```

![](screenshot of error)

#### Explicitly Unwrapping With `!`

The reason that optionals are not immediately compatible with their associated class type is that the optional *could* contain `nil`. In Swift, assigning to `nil` causes a run-time crash. However, there are times when you as the developer can guarantee that an optional will always contain an object of the expected type (and not ever `nil`).

```
class Present {
    var description = "firetruck"
}

class ChristmasTree {
    var presents = [Present?]()
    
    func openNextPresent() -> Present {
        let present = self.presents.popLast()!
        return present!
    }
}
```

// lots and lots of caveats to explicitly unwrapping

// Tim: don't do it unless you have "insider information" that the optional cannot be `nil`, like segue destination view controller, or classes that you've written

The Office, desk prank: https://www.youtube.com/watch?v=A4sNxg1Yv0E 

## (braindump)

- What?
  - If we think about `nil` philosophically, it's a little weird, right? Variables of object type can point to either an instance or ... nothing. We can't have a primitive nothing... weird.
  - Idea behind optionals: the *type* of a variable should indicate whether it can be nil, not whether it's an object or a primitive/value.

- Why?
  - "The Billion Dollar Mistake": http://lambda-the-ultimate.org/node/3186
  - Philosophically cleaner -- we can now have the absence of an integer, or the absence of a struct.
  - nils can't hide -- you *have* to deal with them wherever they may pop up, since `MyObject` and `MyObject?` are different types
  - You literally cannot pass nil to a method unless the programmer has explicitly allowed you to do it

- How?
  - Question mark after the type -- `String?`. This is *not* the same type as `String` -- you cannot pass an optional where a non-optional is requested.
  - May also see `Optional<T>`, like `Optional<String>`

- How to deal with optionals... 
  - To convert back to an actual, non-optional value is called "unwrapping". Show `if let` and `!` and `as!` (tons of caveats on the `!` forms -- they will crash if the optional is actually nil)
  - Alternative: optional chaining with `?.`
  - Talk about `guard`?

Other thoughts:
- I would skip talking about implicitly unwraped optionals. They're poo and are hopefully going the way of the dodo.
