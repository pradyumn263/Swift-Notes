# The Swift Programming Language

# The Basics

## Type Alias

```swift
typealias AudioSample = UInt32

var maxAmplifiedSound = AudioSample.min
```

## Tuples

Tuples group multiple values into a single compound value. The values within a tuple can be of any type and don't have to be of the same type as each other

```swift
let http404Error = (404, "Not Found")
```

Decomposing tuple contents into separate constants or variables, which can be accessed as usual

```swift
let (statusCode, statusMessage) = http404Error
```

Also, we can access individual element values in a tuple using index numbers starting at zero

```swift
print("The status code is \(http404Error.0)")
print("The status message is \(http404Error.1)")
```

Individual elements of the tuple can also be named, and hence we can use them using the defined name

```swift
let http200Status = (statusCode: 200, description: "OK")

print("The status code is \(http200Status.statusCode)"
```

Tuples are useful for simple groups of related values, and they are not suited to the creation of complex data structures. If your data structure is likely to be more complex, use a class or a structure.

## Optionals

These can be used to cope with times when value isn't present. If a value is present then the optional can be unwrapped to access the value, or there isn't a value at all.

Int in Swift has an initializer which tries to convert a String value into an Int, however, not every string can be converted into an Int. 

```swift
let possibleNumber = "123"
let convertedNumber = Int(possibleNumber)
```

In the above code, converted number is inferred to be of type Int? or Optional Int

We can set the value of an optional variable to nil. This cannot be done with non-optional variables.

In Swift, nil is different as NULL in C. Because in Swift, any optional variable can be assigned the value of nil, and not just Objects etc.

### If statements and forced unwrapping

We can use if statement to check if an optional variable has a value, and then force unwrap it to use it's value.

```swift
if convertedNumber != nil {
	print(convertedNumber!)
}
```

Trying to use ! to access a nonexistent optional value triggers a runtime error. Always make sure that an optional contains a non-nil value before using ! to force-unwrap it's value

### Optional Binding

We can rewrite the above example as follows:

```swift
if let actualNumber = Int(possibleNumber) {
	print("The number is \(actualNumber)")
} else {
	print("It is not a valid number")
}
```

## Error Handling

→ Functions can throw an error, which then it's caller can catch and try to handle the error.

```swift
func canThrowAnError() throws {
	//May or maynot throw an error 
}

do {
	try canThrowAnError()
	//No error was thrown
} catch {
	//An error was thrown
}
```

Multiple catch blocks can be used to catch errors from a single try. The do statement creates a new containing scope, which allows errors to be propagated to one or more catch clauses.

```swift
func makeASandwich() throws {
	//..
}

do {
	try makeASandwich()
	eatASandwich()
} catch SandwichError.outOfCleanDishes {
	washDishes()
} catch SandwichError.missingIngredients(let ingredients) {
	buyGroceries(ingredients)
}
```

## Assertions and Preconditions

Assertions and Preconditions are checks that happen at runtime. We can use them to make sure an essential condition is satisfied before executing any further code. If it evaluates to true, code execution continues as usual, if it evaluates to false, the current state of the program is invalid, and code execution ends and app is terminated.

### Debugging with Assertions

```swift
let age = -3
assert(age >= 0, "A person's age can't be less than zero")
```

If the code already checks for the condition, then we can use the **assertionFailure** function to indicate that an assertion has failed.

```swift
if age > 10 {
	print("Hey!")
} else if age >= 0 {

} else {
	assertionFailure("A person's age can't be negative")
}
```

### Enforcing Preconditions

Use a precondition whenever a condition has the potential to be false, but must definitely be true for your code to continue execution. 

```swift
precondition(index > 0, "Index must be greater than Zero")
```

---

# Basic Operators

## Unary Minus Operator

```swift
let three = 3
let minusThree = -three
```

## Comparison Operators

- Equal to (a == b)
- Not equal to (a ! = b)
- Greater than a > b
- Less than a < b
- Greater than or equal to a > = b
- Less than or equal to a < = b

Swift also provides two identity operators "===" and "! ==", which we can use to test whether two object references both refer to the same object instance.

We can compare two tuples if they have the same type and the same number of values. Tuples are compared from left to right, one value at a time, until the comparison finds 2 values that aren't equal. Then those two values are compared, and the result of that comparison determines the overall result of the tuple comparison.

### Nil-Coalescing Operator

It unwraps an optional if it contains a value, or returns the default value if the first one is nil.

```swift
a ?? b
// If a doesnt have value, then default value of b is used
```

## Range Operators

### Closed Range Operators

```swift
for index in 1...5 {
	// 1, 2, 3, 4, 5
}
```

### Half Open Range Operator

```swift
for index in 1..<5 {
	//1, 2, 3, 4
}
```

### One-Sided Ranges

```swift
let names = ["Anna", "Alex", "Brian", "Jack"]

for name in names[2...] {
	print(name)
}
// Brian
// Jack

for name in names[...2] {
	print(name)
}
// Anna 
// Alex
// Brian
```

## Logical Operators

```swift
Logical NOT !a
Logical AND a && b
Logical OR a || b
```

---

# Strings and Characters

### Initializing an Empty String

```swift
var emptyString = ""
var anotherEmptyString = String()

if emptyString.isEmpty {
	print("String is empty")
}
```

### String Mutability

```swift
var variableString = "Horse" // Mutable 
let constantString = "and Tatti" // Immutable
```

Strings in Swift are Value type. If we create a new String value, that value is copied when it's passed to a function or method or when it is assigned to a constant or a variable. 

### Working with characters

```swift
for character in "Dog!🐶" {
	print(character)
}
```

String values can also be constructed by passing an array of character values as an argument to it's initializer

```swift
let catCharacters: [Character] = ["C", "a", "t", "!", "🐈"]

let catString = String(catCharacters)
print(catString)
```

### Counting Characters

To retrieve a count of the Character values in a String, we can use the count property of the string

```swift
let exampleString = "This is an Example string"
print(exampleString.count)
```

### Accessing and Modifying a String

**Why are Swift Strings not indexed by Integer Values?** 
Different characters can require different amounts of memory to store. So, in order to determine which Character is at a particular position, we need to iterate over each Unicode scalar from the start or end of the String. Hence, Swift Strings can not be indexed by Integers

To access the indices before and after a given index, we can use the index(before:) and index(after:) methods of the String.

```swift
let greeting = "Guten Tag!" 

greeting[greeting.startIndex] // -> G

greeting[greeting.index(before: greeting.endIndex)] // -> !

greeting[greeting.index(after: greeting.startIndex)] //-> u

let index = greeting.index(greting.startIndex, offsetBy: 7)

greeting[index] // -> a
```

To Iterate over all the characters of the string one by one, for in loop can be used as Follows!

```swift
for index in greeting.indices {
	print(greeting[index])
}
```

**Where can we use the startIndex and endIndex properties in Swift?**
We can use the startIndex, endIndex and index(before: ) and index(after: ) methods on any type that conforms to the Collection protocol. This includes String, as well as types such as Arrays, Dictionary and Set.

### Inserting and Removing

→ To insert a single character into a string at a specified index, we can use the insert(_:at: ) method.

→ To insert the contents of another string at a specified index, we can use the 
     insert(contentsOf: at: ) method

→ To remove a single character from a string at a specified index, use the remove(at: ) method, and to remove a substring at a specified range, use the removeSubrange( _ : ) method

```swift
var welcome = "hello"
welcome.insert("!", at: welcome.endIndex)
// welcome now equals "hello!"

welcome.insert(contentsOf: " there", at: welcome.index(before: welcome.endIndex))
// welcome now equals "hello there!"

welcome.remove(at: welcome.index(before: welcome.endIndex))
// welcome now equals "hello there"

let range = welcome.index(welcome.endIndex, offsetBy: -6)..<welcome.endIndex
welcome.removeSubrange(range)
// welcome now equals "hello"
```

### Substrings

**What is the difference between Strings and Substrings in Swift?**
As a performance optimization, a substring can reuse part of the memory that's used to store the original string, or part of the memory that is used to store another substring.

**Why are substrings not suited for long term storage?** 
They reuse the storage of the original string, the entire original string must be kept in memory as long as any of it's substrings are being used

![The%20Swift%20Programming%20Language%200f240bf1034149238d803653ed39a8c9/Screenshot_2020-10-22_at_5.33.12_PM.png](The%20Swift%20Programming%20Language%200f240bf1034149238d803653ed39a8c9/Screenshot_2020-10-22_at_5.33.12_PM.png)

Both String and Substring conform to the StringProtocol, which means it's often convenient for string-manipulation functions to accept a StringProtocol value. You can call such functions with either a String or a Substring value.

### Comparing Strings

Swift provides three ways to compare textual values: String and Character equality, prefix Equality, and suffix equality.

**Prefix and Suffix Equality**

To check whether a string has a particular string prefix or suffix, call the string's hasPrefix( _ : ) and hasSuffix( _ : ) methods, both of which take a single argument of type String and return a Boolean value

---

# Collection Types

Swift provides 3 primary collection types, known as arrays, sets and dictionaries for storing collections of values. 

**Arrays** → Ordered collections of values

**Sets** → Unordered collections of unique values

**Dictionaries** → Unordered collections of key-value associations