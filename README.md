# Swift — Optionals

## Objectives

1. Conceptualize the nature and necessity of `nil`, the value of "nothing".
2. Distinguish a class from its optional type with the suffix `?`.
3. Unwrap optionals using `if let`.
4. Explicitly unwrap optionals using `!`.
5. Recognize commons errors that `nil` and optionals can cause in Swift.

## Introduction

>I call it my billion-dollar mistake. It was the invention of the null reference in 1965. At that time, I was designing the first comprehensive type system for references in an object oriented language (ALGOL W). My goal was to ensure that all use of references should be absolutely safe, with checking performed automatically by the compiler. But I couldn't resist the temptation to put in a null reference, simply because it was so easy to implement. This has led to innumerable errors, vulnerabilities, and system crashes, which have probably caused a billion dollars of pain and damage in the last forty years.  
—[Tony Hoare](https://en.wikipedia.org/wiki/Tony_Hoare), *on including the [null reference](https://en.wikipedia.org/wiki/Null_pointer) in the ALGOL W programming language*

#### `nil`: Something From Nothing

There's an old riddle that asks, "what can you put in a barrel to make it lighter?" While there's a [variety of answers](https://answers.yahoo.com/question/index?qid=20080303110423AA6pGbZ) that could apply, the classic answer is "a hole." The adage perfectly encapsulates the irony of conceptualizing nothingness as *some*thing (i.e. being a hole).

How can nothingness be something? Perhaps only as a result of human abstraction. However, the *idea* of nothingness is actually something—it's an idea! In a metaphysical movie-ending twist, absence becomes presence.

Like [the Ringwraiths](https://books.google.com/books?id=MSSXAAAAQBAJ&pg=PA34&lpg=PA34&dq=nazgul+nothingness&source=bl&ots=v9GEiGSk9S&sig=tHJiG0t6mZHNKx8rFuCl40t0yZA&hl=en&sa=X&ved=0CEoQ6AEwB2oVChMImtKu5rDgyAIVCnY-Ch3tBw0W#v=onepage&q=nazgul%20nothingness&f=false) in *The Lord of the Rings*, many programming languages include a representation of nothingness — that ancient, sleeping evil with names known to all: `nil`, `(null)`, `NSNull()`, `∅` ("nought"), and the [Witch-King of Angmar](http://lotr.wikia.com/wiki/Witch-king_of_Angmar).

![](https://curriculum-content.s3.amazonaws.com/swift/swift-optionals-reading/moonlight-ringwraith.jpg)  
—The Nazgúl appear darker than the black of the night, screenshot from *The Lord of the Rings (2001)*.

But if `nil` is so evil, then why was it included in any programming language to begin with? That's been [a topic of much debate](http://programmers.stackexchange.com/a/237699) in programming circles for decades; but for as much as it causes problems, it does have its uses:

##### Search Results

Image that you're playing a game of [Go Fish](http://www.gofish-cardgame.com/), which is a card game that involves the active player asking the other player for cards that match a certain rank. If an inactive player possesses no matching cards of that rank, they communicate this by saying "Go Fish" meaning "I have nothing to give you." To represent this case in an application, we could have the inactive player return "nothing" in order to communicate that there were no matches to the active player's search criteria.

When working in code and accessing a collection which may or may not be empty, it's useful to have the option of not returning anything. But this result of nothingness has to be communicated in some way. Either we have to prevent our code from attempting to access the nothingness altogether (such as with `Array`'s `removeLast()` method requiring that the `count` property be greater than `0`), or we give "nothing" some physically embodied form and define a behavior for handling the case when we only have "nothing" to give back. This "physical form of nothing" is essentially what the various forms of `nil` are meant to represent.

##### Non-essential Values

It may also be the case that you want to hold information fields (instances) that are not necessarily filled in, such as non-essential information fields for a user account:

![](https://curriculum-content.s3.amazonaws.com/swift/swift-optionals-reading/learn_signup.png)  
—From the *Learn.co* sign-up page.

In order to hold the potential that an information field is not filled out (i.e. set to a value of "nothing"), the user's non-essential properties must be able to hold a "nothing" value.

#### `nil` in Swift

The Swift programming language is a [strongly typed](https://en.wikipedia.org/wiki/Strong_and_weak_typing) language, meaning that object typing is enforced during the compile stage in order to reduce the risk of errors during run-time. However, even in most strongly typed languages, it is valid to assign `nil` to most variables. In Objective-C, for instance:

```objc
NSString *myString = @"Hello";
myString = nil;     // Totally valid
```

That is, even though `myString` is defined to be an `NSString`, it can really be either a string or nil.

Swift takes type-safety a step further — it treats `nil` [as its own special type](http://stackoverflow.com/questions/24043589/null-nil-in-swift-language), and the Swift compiler will not allow to assign it to just any variable.

```swift
var twitter: String = nil     // error
```

![](https://curriculum-content.s3.amazonaws.com/swift/swift-optionals-reading/assign_nil_to_non_optional_error.png)

So, how can we assign the absence of a value to an instance that *cannot* contain "nothing"? Enter **Optionals**.

## Optionals

If you need a variable that can be either a value or `nil`, its type needs to reflect that. Swift handles this distinction by appending a `?` ("question mark") to the end of the class name. So, if we have a class `SomeClass`, its optional type will be represented as `SomeClass?`. A variable of type `SomeClass` can *only* hold instances of that class. A variable of type `SomeClass?` can hold an instance or `nil`. 

The "optional" types provide a wrapper for the class that can encapsulate `nil`. They essentially read as *an-instance-of-SomeClass-or-nil*, as opposed to the normal *definitely-and-only-an-instance-of-SomeClass*. It's important to understand the distinction here: **a class's type and its optional type are not the same type.** An instance of the `SomeClass?` optional *cannot* be assigned to an instance of `SomeClass` without first being "unwrapped" from the potential that it actually contains `nil`. The term "unwrapping" effectively means "handling the `nil` case".

The optional is defining the expectation that an object will be of a specific class but without committing to it actually having any contents. It's like having presents under a Christmas tree before Christmas Day: there's every expectation that underneath the colorful box-shaped pieces of wrapping paper there resides a plethora of highly-anticipated presents. However, without being able to open those presents until the appropriate time, we can't be *certain* that within the paper there is actually anything other than thin air. For all that we explicitly know until Christmas morning, each one of those presents could actually be a cruel trick:

![](https://curriculum-content.s3.amazonaws.com/swift/swift-optionals-reading/wrappingpaperdesk.gif)  
—Jim pranking Dwight in *The Office*. Don't be like Dwight.

Like opening the box that holds [Schrödinger's cat](https://youtu.be/itQVDA6_TME?t=1m27s), in order to certify that an optional does *not* contain `nil` we first have to "unwrap" it.

### Unwrapping Optionals

In Swift, the term "unwrapping" means the process of extracting an actual value from an optional variable. It also is the way in which Swift requires developers to think of the `nil` case, defining a behavior in the case that an instance has nothing in it.

There are a variety of means to do this in Swift—the two most basic of which are below:

#### Unwrapping With `if let`

Assuming that we have a user's information represented like this:

```swift
let firstName = "Jenny"
let lastName = "Curran"
let email = "jenny@provider.net"
let phone: String? = "867-5309"
let city: String? = "New York"
let twitter: String? = nil
```
We could try to utilize the optional values directly:

```swift
print("First Name: \(firstName)")
print("Last Name: \(lastName)")
print("Email: \(email)")
print("Phone: \(phone)")
print("City: \(city)")
print("Twitter: \(twitter)")
```

However, this will treat the `phone`, `city`, and `twitter` instances like the optionals that they are, printing:

```
First Name: Jenny
Last Name: Curran
Email: jenny@provider.net
Phone: Optional("867-5309")
City: Optional("New York")
Twitter: nil
```

Remember—a `String?` is not a `String`, so it doesn't print like one.

However, we can use an `if let` statement to unwrap the optionals in order to access the strings they contain. An `if let` statement detects whether the optional contains `nil` or an object. If it contains an object, it creates the constant defined in the `if let` statement of the class type that the optional is on and runs the code inside the `if let`'s scope.

In our case, we can use the `if let` statement to "mask" or "shadow" the constant with an object value as long as it doesn't contain `nil`:

```swift
print("First Name: \(firstName)")
print("Last Name: \(lastName)")
print("Email: \(email)")
if let phone = phone {
    print("Phone: \(phone)")
}
if let city = city {
    print("City: \(city)")
}
if let twitter = twitter {
    print("Twitter: \(twitter)")
}
```
This will instead print:

```
First Name: Jenny
Last Name: Curran
Email: jenny@provider.net
Phone: 867-5309
City: New York
```
Notice that the "Twitter" field did not print. This is because the `if let` failed since the optional contained `nil`. We could set up `else` statements to define a behavior for when the optional contains `nil`:

```swift
print("First Name: \(firstName)")
print("Last Name: \(lastName)")
print("Email: \(email)")

if let phone = phone {
    print("Phone: \(phone)")
} else {
    print("--no phone number--")
}

if let city = city {
    print("City: \(city)")
} else {
    print("--no city--")
}

if let twitter = twitter {
    print("Twitter: \(twitter)")
} else {
    print("--no twitter handle--")
}
```
This will instead print:

```
First Name: Jenny
Last Name: Curran
Email: jenny@provider.net
Phone: 867-5309
City: New York
--no twitter handle--
```

Since the `twitter` optional was not set and contained `nil`, the `if let` statement failed causing its `else` statement to run. 

Can you see how optional unwrapping forced us to make a better program? It required us to think about what we should do when the instances are `nil`. That's the power and goal of Optionals in Swift—that it allows significantly less ambiguity in our code and reduces the problems caused by `nil`.

![](https://curriculum-content.s3.amazonaws.com/swift/swift-optionals-reading/the-lord-of-the-rings-aragorn-vs-nazgul-o.gif)  
—Aragorn, unwrapping a `nil` optional.

#### Explicitly Unwrapping With `!`

There are times when you as the developer can *guarantee* that an optional will *always* contain an object of the expected type (and not ever `nil`). In these rare situations it is a lot cleaner to explicitly tell the compiler that you're *certain* the optional contains an object. **Be careful with this syntax because if you're wrong, you'll experience a run-time crash.**

Explicit unwrapping of optionals is performed using a **trailing** `!` ("exclamation point"). **Note:** *A leading* `!` *is the "not" operator which inverts a boolean.* It can be placed **after** an instance to unwrap it:

```swift
let phone: String? = "867-5309"
let phoneStr = phone!
print("Phone: \(phoneStr)")
```
This will print: `Phone: 867-5309`.

Or placed after a method call to unwrap a returned optional:

```swift
let age = "30"
let ageInt = Int(age)!
print("Age: \(ageInt)")
```
This will print: `Age: 30`.

However, if you attempt to explicitly unwrap an optional that contains `nil` you'll cause a run-time error:

```swift
var christmasPresents: [String?] = ["firetruck", "spaceship", nil]

let firetruck: String? = christmasPresents.removeFirst()
print(firetruck!)

let spaceship: String? = christmasPresents.removeFirst()
print(spaceship!)

let nothing: String? = christmasPresents.removeFirst()
print(nothing!)    // error
```

This will print:

```
firetruck
spaceship
```
And then promptly crash.

![](https://curriculum-content.s3.amazonaws.com/swift/swift-optionals-reading/explicity_unwrap_nil_optional_error.png)

*With great power comes great responsibility.* In general, it's best to avoid using `!` to unwrap optionals.


<p data-visibility='hidden'>View <a href='https://learn.co/lessons/swift-optionals-reading' title='Swift Optional Reading'>Swift Optional Reading</a> on Learn.co and start learning to code for free.</p>
