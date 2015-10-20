# Optionals in Swift

# Objectives

(braindump)

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
