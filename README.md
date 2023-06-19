# Guidelines for Developing iOS Applications with Swift

Slightly modified guidelines from raywenderlich.com

## Table of Contents

* [Let's Start with Perfectionism](#let's-start-with-perfectionism)
* [Page Width](#page-width)
* [Naming](#naming)
* [Methods](#methods)
* [Booleans](#booleans)
* [Class Prefixes](#class-prefixes)
* [Delegates](#delegates)
* [Type-Dependent Context](#type-dependent-context)
* [Generics](#generics)
* [Language](#language)
* [Code Organization](#code-organization)
* [Protocol Conformance](#protocol-conformance)
* [Unused Code](#unused-code)
* [Minimal Imports](#minimal-imports)
* [Indentation](#indentation)
* [Comments](#comments)
* [Classes and Structures](#classes-and-structures)
* [Using self](#using-self)
* [Computed Properties](#computed-properties)
* [Using final](#using-final)
* [Function Declaration and Invocation](#function-declaration-and-invocation)
* [Closure Expressions](#closure-expressions)
* [Types](#types)
* [Constants](#constants)
* [Static Methods and Type Properties](#static-methods-and-type-properties)
* [Optionals](#optionals)
* [Lazy Initialization](#lazy-initialization)
* [Type Inference](#type-inference)
* [Syntactic Sugar](#syntactic-sugar)
* [Arrays and Dictionaries](#arrays-and-dictionaries)
* [Functions vs. Methods](#functions-vs-methods)
* [Memory Management](#memory-management)
* [Object Lifetime Extension](#object-lifetime-extension)
* [Access Control](#access-control)
* [Control Flow](#control-flow)
* [The Golden Path](#the-golden-path)
* [Using guard](#using-guard)
* [guard Handling](#guard-handling)
* [Semicolons](#semicolons)
* [Parentheses](#parentheses)
* [Organization Format and Bundle Identifier](#organization-format-and-bundle-identifier)
* [References](#references)

## Let's Start with Perfectionism

As banal as it may sound, always strive to compile your code without warnings.

## Page Width

Set the page width to 120 characters (Xcode / Preferences / Text Editing / Page guide at column). This value is recommended to be included as a rule in SwiftLint. This way, all code will fit within the specified page width and be more readable.

## Naming

Proper naming simplifies code reading and understanding. Use Swift naming conventions described in the [API Design Guidelines](https://swift.org/documentation/api-design-guidelines/). Some key features include:

- Aim for clarity in function calls
- Choose clarity over brevity
- Use camelCase, not snake_case
- Use uppercase for types and protocols, lowercase for everything else
- Include only necessary words, avoiding excessive ones
- Use role-based naming, but make the object's type clear from its name (e.g., `contactsView`, `nameLabel`, `descriptionViewHeightConstraint`)
- Sometimes provide contextual type information with weakly typed types
- Strive for fluid use
- Start factory methods with "make" (e.g., `makeProduct`)
- Naming methods:
- Verb methods follow the -ed, -ing rule for immutable objects (e.g., `sorted`)
- Verb methods use the base form for mutable objects (e.g., `sort`)
- Boolean types should read like assertions (e.g., `isEnabled`)
- Protocols should be named as nouns (e.g., Sphere -> `Spherable`)
- Protocols describing capability should end in -able or -ible (e.g., `Codable`)
- Use terms that won't surprise experts or confuse beginners
- Avoid abbreviations
- Provide weighty reasons for name choices
- Prioritize methods and properties over free functions
- Use abbreviations in either upper or lower case (e.g., `userID`, `isRepresentableAsASCII`, `utf8Bytes`)
- Give the same base name to methods that have the same meaning (e.g., don't use `configurate` somewhere and `setup` somewhere else)
- Avoid overloading return types
- Choose self-documenting names for parameters
- Mark parameters if they are closures or tuples
- Use default parameters
- The name of an object should hint at its type, which improves code readability:
- Arrays with a plural suffix (e.g., `names`, `userIDs`, `mainIndexes`)
- Dictionaries with a `Map` suffix (e.g., `userRatingMap`)
- Sets with a `Set` suffix (e.g., `imageSet`)

### Methods

It is very important when it comes to method naming. To refer to a method, it is desirable to use the simplest form possible:

Write the method name without parameters (e.g., `addTarget`)
Write the method name with argument labels (e.g., `addTarget(:action:)`)
Write the full method name with argument labels and types (e.g., `addTarget(_: Any?, action: Selector?)`)
In the above example using _UIGestureRecognizer_, option 1 is more unambiguous and preferred.

**Tip**: You can use the Xcode jump bar to find methods with argument labels.

![Methods in Xcode jump bar](screens/xcode-jump-bar.png)

### Booleans

For Boolean variables, it is customary to use an auxiliary verb at the beginning.

**Recommended:**
```swift
var isFavoriteCategoryFilled: Bool
var hasRoaming: Bool
var isBlocked: Bool
```

**Not Recommended:**
```swift
var favoriteCategoryFilled: Bool
var roaming: Bool
var blocked: Bool
```

### Class Prefixes

Swift types are automatically named after the module they belong to, and you shouldn't add a class prefix such as `DP`. If two names from different modules collide, you can disambiguate by specifying the type name with the module name. Only include the module name if there is a possibility of confusion, which should be extremely rare.

```swift
import SomeModule

let myClass = MyModule.UsefulClass()
```

### Delegates 
When creating custom delegate methods, the first parameter should be the delegate parameter (UIKit provides numerous examples of this).

**Recommended:**
```swift
func namePickerView(_ namePickerView: NamePickerView, didSelectName name: String)
func namePickerViewShouldReload(_ namePickerView: NamePickerView) -> Bool
```

**Not Recommended:**
```swift
func didSelectName(namePicker: NamePickerViewController, name: String)
func namePickerShouldReload() -> Bool
```

### Type-Dependent Context

Utilize the compiler's abilities to produce shorter and more understandable code.

**Recommended:**
```swift
let selector = #selector(viewDidLoad)
view.backgroundColor = .red
let toView = context.view(forKey: .to)
let view = UIView(frame: .zero)
```

**Not Recommended:**
```swift
let selector = #selector(ViewController.viewDidLoad)
view.backgroundColor = UIColor.red
let toView = context.view(forKey: UITransitionContextViewKey.to)
let view = UIView(frame: CGRect.zero)
```

### Generics

Generic names should be clear and in UpperCamelCase format. If a type name doesn't have a significant role, use a traditional single uppercase letter such as T, U, or V.

**Recommended:**
```swift
struct Stack<Element> { ... }
func write<Target: OutputStream>(to target: inout Target)
func swap<T>(_ a: inout T, _ b: inout T)
```

**Not Recommended:**
```swift
struct Stack<T> { ... }
func write<target: OutputStream>(to target: inout target)
func swap<Thing>(_ a: inout Thing, _ b: inout Thing)
```

### Language

Base your spelling on American English according to Apple's API.

**Recommended:**
```swift
let color = "red"
```

**Not Recommended:**
```swift
let colour = "red"
```

## Code Organization

Use extensions to break down your code into logical functional blocks. Each extension should be labeled with a // MARK: - comment to keep everything well-organized.

It is better to follow the following order:

```swift
// MARK: - Types

// MARK: - Constants

// MARK: - IBOutlets

// MARK: - Public Properties

// MARK: - Private Properties

// MARK: - UIViewController(*)

// MARK: - Public Methods

// MARK: - IBActions

// MARK: - Private Methods

```
> (*)Instead of UIViewController, you should write the appropriate superclass from which you are inheriting.


### Protocol Conformance

In particular, when adding protocol conformance, it is desirable to add a separate extension to implement its methods. This keeps the related methods together with the protocol and can simplify the instructions for adding the protocol to a class with its associated methods.

**Recommended:**
```swift
class MyViewController: UIViewController {
// код класса
}

// MARK: - UITableViewDataSource
extension MyViewController: UITableViewDataSource {
// методы датасорса
}

// MARK: - UIScrollViewDelegate
extension MyViewController: UIScrollViewDelegate {
// методы делегата
}
```

**Not Recommended:**
```swift
class MyViewController: UIViewController, UITableViewDataSource, UIScrollViewDelegate {
// все методы
}
```

> Это правило не работает в двух случаях:
> - When you need an overriding method that is implemented in another extension.
> - When you're using a class with generics and Objective-C protocols with additional methods (@objc is not supported in extensions of generic classes or classes that inherit from generic classes).

Since the compiler doesn't allow re-declaration of protocol conformance in a subclass, it is not always necessary to repeat groups of extensions from the base class. This applies if the subclass is final and only a few methods are overridden. The preservation of extension groups is at the discretion of the author.

For UIViewController subclasses, consider breaking them down into separate extensions: lifecycle, custom accessors, and IBActions.

### Unused Code

Unused (dead) code, including Xcode template code and placeholder comments, should be removed. Unless you are writing a tutorial where you provide instructions to the reader to use the commented code.

Methods that are not directly related to the tutorial and whose implementation simply calls the superclass should also be removed. This includes any empty/unused methods in UIApplicationDelegate.

**Recommended:**
```swift
override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
return Database.contacts.count
}
```

**Not Recommended:**
```swift
override func didReceiveMemoryWarning() {
super.didReceiveMemoryWarning()
// Dispose of any resources that can be recreated.
}

override func numberOfSections(in tableView: UITableView) -> Int {
// #warning Incomplete implementation, return the number of sections
return 1
}

override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
// #warning Incomplete implementation, return the number of rows
return Database.contacts.count
}
```
### Minimal Imports

Maintain the minimum number of imports. For example, omit the import of Foundation if you are already importing UIKit.

## Indentation

* Use 4 spaces for indentation instead of tabs to save space and help prevent line wrapping. Make sure to select the appropriate option in Xcode (Preferences / Text Editing / Indentation / Prefer Indent using: Spaces, and Tab width: 4).

* Curly braces (in if/else/switch/while, etc.) always open on the same line as the statement but close on a new line.tch`/`while` и т.д.) всегда открываются в той же строки, что и оператор, но закрываются на новой строке

* Tip: You can automatically apply proper indentation by selecting the desired code and then pressing Control-I (or Editor/Structure/Re-Indent).

**Recommended:**
```swift
if user.isHappy {

} else {

}
```

**Not Recommended:**
```swift
if user.isHappy
{

}
else {

}
```

* There should be exactly one empty line between methods to aid visual clarity and organization. Blank lines within methods should separate functionality, but having too many of them within a method often indicates the need for refactoring.

* Colons always have no space to the left but one space to the right. The exceptions are the ternary operator `? :`, an empty dictionary `[:]`, and the syntax `#selector` for unnamed parameters `(_:)`.

**Recommended:**
```swift
class TestDatabase: Database {
var dataMap: [String: CGFloat] = ["A": 1.2, "B": 3.2]
}
```

**Not Recommended:**
```swift
class TestDatabase : Database {
var dataMap :[String:CGFloat] = ["A" : 1.2, "B":3.2]
}
```
* Ideally, long lines should be within 70 characters. However, there is no strict limit.
* There should be no trailing whitespace at the end of lines.
* There should be one empty line at the end of each file.

## Comments

When necessary, use comments to explain why a certain piece of code does something. Comments should be updated to reflect the current state of the code or removed entirely.

Avoid comments within a code block, as the code should be self-explanatory as much as possible. Exception: This does not apply to comments used for documentation purposes.

## Classes and Structures

### When to Use?

Structures have [value semantics](https://developer.apple.com/library/mac/documentation/Swift/Conceptual/Swift_Programming_Language/ClassesAndStructures.html#//apple_ref/doc/uid/TP40014097-CH13-XID_144). Use structures for things that don't have an identity. An array containing [a, b, c] is truly the same as another array containing [a, b, c], and they are completely interchangeable. It doesn't matter whether you use the first array or the second because they represent the same thing. That's why arrays are structures.

Classes have [reference semantics](https://developer.apple.com/library/mac/documentation/Swift/Conceptual/Swift_Programming_Language/ClassesAndStructures.html#//apple_ref/doc/uid/TP40014097-CH13-XID_145). Use classes for things that have an identity or a specific lifecycle. You can model a person as a class because objects of two different people are two different things. It's because if two people have the same name and birth date, it doesn't mean they are the same person. But a person's birth date would be a structure because the date March 3, 1950, is the same as any other March 3, 1950 date object. The date itself doesn't have an identity.

Sometimes things need to be structures but also need to conform to `AnyObject` or have been historically modeled as classes (`NSDate`, `NSSet`). Try to closely follow these recommendations whenever possible.

### Example Definition

Here's an example of a well-designed class definition:

```swift
class Circle: Shape {
var x: Int, y: Int
var radius: Double
var diameter: Double {
get {
return radius * 2
}
set {
radius = newValue / 2
}
}

init(x: Int, y: Int, radius: Double) {
self.x = x
self.y = y
self.radius = radius
}

convenience init(x: Int, y: Int, diameter: Double) {
self.init(x: x, y: y, radius: diameter / 2)
}

override func area() -> Double {
return Double.pi * radius * radius
}
}

extension Circle: CustomStringConvertible {
var description: String {
return "center = \(centerString) area = \(area())"
}
private var centerString: String {
return "(\(x),\(y))"
}
}
```

The above example demonstrates the following styles:

- Specify types for properties, variables, constants, argument declarations, and other operators with a space after the colon, such as `x: Int` and `Circle: Shape`.
- Define multiple variables and structures in a single line if they have a common purpose/context.
- Definition of getter and setter, as well as property observers.
- Do not add access modifiers, such as `internal`, if they are already set by default. Similarly, do not repeat the access modifier when overriding a method.
- Organize additional functionality (e.g., printing) in extensions.
- Hide implementation details not related to public access, such as `centerString`, inside an extension using private access control.

### Self usage

To keep the code concise, avoid using `self` unless it is required by the compiler. Swift does not require accessing object properties or calling its methods using `self`.

Use `self` only when it is necessary for disambiguating properties from arguments in escaping closures or in initializers. In other words, if the code compiles without `self`, omit it.

### Computed properties

If a computed property is read-only, omit the get keyword. The get keyword is only required when a set clause is provided.

**Recommended:**
```swift
var diameter: Double {
return radius * 2
}
```

**Not Recommended:**
```swift
var diameter: Double {
get {
return radius * 2
}
}
```

Всегда используйте вычисляемое свойство вместо метода без аргументов и побочных эффектов.

**Recommended:**
```swift
var homeFolderPath: Path {
//...
return path
}
```

**Not Recommended:**
```swift
func homeFolderPath() -> Path {
//...
return path
}
```

### Using Final

Marking classes as final in examples can be distracting from the main topic and is not always necessary. However, using final can sometimes clarify your intentions and is worth the cost. In the example below, Box has a specific purpose, and extension as a derived class is not intended. Marking it as final makes this clearer.

```swift
final class Box<T> {
let value: T
init(_ value: T) {
self.value = value
}
}
```

Additionally, it improves code readability and speeds up compilation.

## Function Declaration and Invocation

Keep short function or type declarations on a single line, including the opening curly brace:

```swift
func method(parameter: [Double]) -> Bool {
// ...
}
```

For functions with long argument lists, use line breaks for each argument, including the first one, if they exceed the page width guideline. To visually separate the arguments from the body of the method (since they are indented), also move the closing parenthesis to a new line:

```swift
func method(
parameter1: [Double], 
parameter2: Double,
parameter3: Int
) -> Bool {
// ...
}
```

The same rule applies to function invocations:

```swift
let result = method(
parameter1: parameter1, 
parameter2: parameter2,
parameter3: parameter3
)
```

Leave one blank line between functions, and one before and after MARK:

```swift
func colorView() {
//...
}

func reticulate() {
//...
}

// MARK: - Private Methods

private func retry() {
//...
}
```

## Closure Expressions

Use trailing closure syntax only if there is a single closure at the end of the argument list. Provide a clear name for the closure. The type and parentheses of the closure should be omitted.

**Recommended:**
```swift
UIView.animate(withDuration: 1.0) {
// ...
}

UIView.animate(
withDuration: 1.0,
animations: {
// ...
},
completion: { finished in
// ...
})
```

**Not Recommended:**
```swift
UIView.animate(withDuration: 1.0, animations: {
// ...
})

UIView.animate(withDuration: 1.0, animations: {
// ...
}) { (f: Bool) in
// ...
}
```

For single-expression closures where the context is clear, use implicit returns:

```swift
participants.sort { a, b in
a > b
}
```

Chained methods using trailing closures should be readable and easy to understand. For example:

```swift
let value = numbers
.map { $0 * 2 }
.filter { $0 > 50 }
.map { $0 + 10 }
```
## Types

Always use native Swift types when available. Swift provides bridging to Objective-C types, so you can use the full range of methods as needed.

**Recommended:**
```swift
let width: Double = 120 // Double
let widthString = (width as NSNumber).stringValue // String
```

**Not Recommended:**
```swift
let width: NSNumber = 120.0 // NSNumber
let widthString: NSString = width.stringValue // NSString
```

In SpriteKit code, use CGFloat as it makes the code more concise, avoiding excessive type conversions.

### Constants

Constants are defined using the let keyword, and variables are defined using var. Always use let instead of var if the value of the variable won't change.

**Tip**: There's a nice technique where you define objects using let and change it to var if the compiler complains.

You can define constants for a type, rather than an instance of that type, using type properties. To declare a type property as a constant, simply use static let. Type properties declared this way are much better than global constants because they are easier to distinguish from instance properties. For example:

**Recommended:**
```swift
enum Math {
static let e = 2.718281828459045235360287
static let root2 = 1.41421356237309504880168872
}

let hypotenuse = side * Math.root2
```

**Not Recommended:**
```swift
let e = 2.718281828459045235360287  // портит глобальное пространство имен
let root2 = 1.41421356237309504880168872

let hypotenuse = side * root2 // что за root2?
```

If you must use the global namespace, consider naming the constants in the following format:

**Recommended:**
```swift
let TopMargin: CGFloat = 10
```

**Not Recommended:**
```swift
let kTopMargin: CGFloat = 10
let bottomMargin: CGFloat = 100 
```

### Static Type Methods and Properties

Static methods and properties work similarly to global functions and global variables and should also be used sparingly. They are useful when functionality is tied to a specific type or when compatibility with Objective-C is required.

### Optional Values

Variables and return types of functions can be declared as optionals using ?, where nil is allowed as a value.

Use implicitly unwrapped types declared with ! only when it's known that they will be initialized before being used. For example, subviews that will be set up in viewDidLoad.

When accessing an optional value, use optional chaining:

```swift
self.textContainer?.textLabel?.setNeedsDisplay()
```

Also, use optional binding if let when it is more convenient to unwrap the value and perform multiple operations:

```swift
if let textContainer = self.textContainer {
// ...
}
```

When naming optional variables and properties, avoid indicating their optionality, such as optionalString or maybeView, as their optionality is already indicated in the type declaration.

For implicitly unwrapped optional types, avoid using names like unwrappedView or actualLabel. Also, consider adding a new line after if when there are multiple variables in the unwrapping section.

**Recommended:**
```swift
var subview: UIView?
var volume: Double?

// later...
if 
let subview, 
let volume {
// ...
}
```

**Not Recommended:**
```swift
var optionalSubview: UIView?
var volume: Double?

if 
let unwrappedSubview = optionalSubview {
if let realVolume = volume {
//...
}
}
```
### Lazy Initialization

Consider using lazy initialization for finer control over the lifetime of an object. This is especially relevant for UIViewController, which lazily loads views. You can use a closure that is immediately invoked { }() or invoke a factory method. Here's an example:

```swift
lazy var locationManager: CLLocationManager = self.makeLocationManager()

private func makeLocationManager() -> CLLocationManager {
let manager = CLLocationManager()
manager.desiredAccuracy = kCLLocationAccuracyBest
manager.delegate = self
manager.requestAlwaysAuthorization()
return manager
}
```

**Note:**
- `[unowned self]` is not required here. There is no retain cycle being created.
- The location manager has a side effect of presenting a user interface to ask for permission, so having this level of fine control makes sense in this case.

### Type Inference

Always prefer concise code and let the compiler infer the type for constants or variables of individual instances. Type inference is also suitable for small arrays and dictionaries. If necessary, you can specify a specific type, such as CGFloat and Int16.

**Recommended:**
```swift
let message = "Click the button"
let currentBounds = computeViewBounds()
var names = ["Mic", "Sam", "Christine"]
let maximumWidth: CGFloat = 106.5
```

**Not Recommended:**
```swift
let message: String = "Click the button"
let currentBounds: CGRect = computeViewBounds()
let names = [String]()
```

#### Type Annotation for Empty Arrays and Dictionaries

Type annotation is not necessary for empty arrays and dictionaries

**Recommended:**
```swift
var names: [String] = []
var lookupMap: [String: Int] = [:]
```

**Not Recommended:**
```swift
var names = [String]()
var lookupMap = [String: Int]()
```

### Syntactic Sugar

Use the shorthand versions of type declarations using generics syntax.

**Recommended:**
```swift
var deviceModels: [String]
var employeeMap: [Int: String]
var faxNumber: Int?
```

**Not Recommended:**
```swift
var deviceModels: Array<String>
var employeeMap: Dictionary<Int, String>
var faxNumber: Optional<Int>
```

### Arrays and Dictionaries

Use the following line break when an array or dictionary exceeds the width of the page:

**Recommended:**
```swift
let deviceModelMap = ["BMW": 100, "Mercedes": 120]
let wheels = [
    "Continental", 
    "Vitoria", 
    ...
]
```

**Not Recommended:**
```swift
let wheels = ["Continental", "Vitoria", // параметры, превышающие ширину страницы]
```

## Functions vs Methods

Free functions, which are not tied to a class or type, should be used with caution. When possible, use methods instead of free functions. This helps with readability and discoverability.

Free functions are most suitable when they are not tied to any specific type or instance.

**Recommended:**
```swift
let sortedItems = items.mergeSorted()  // легко найти
rocket.launch()  // воздействует только на модель
```

**Not Recommended:**
```swift
let sortedItems = mergeSort(items)  // сложно найти
launch(&rocket)
```

**Исключения для свободных функций**
```swift
let tuples = zip(a, b)
let value = max(x, y, z)
```

## Memory Management

Code should not create retain cycles. Analyze the object graph (Xcode / Debug navigator / View Memory Graph Hierarchy) and prevent strong reference cycles by using weak or unowned references. Additionally, value types (struct, enum) can be used to avoid cycles.

### Retaining Objects

Retain objects using [weak self] and guard let self = self else { return } (if necessary). It is better to use [weak self] rather than [unowned self] as it is not always certain that self will be alive at that point. Retaining objects is the recommended approach for optional unwrapping.

**Recommended:**
```swift
resource.request().onComplete { [weak self] response in
    guard let self = self else { return }
    let model = self.updateModel(response)
    self.updateUI(model)
}
```

**Not Recommended:**
```swift
// будет сбой, если self будет освобожден (выгружен из памяти) в момент выполнения замыкания
resource.request().onComplete { [unowned self] response in
    let model = self.updateModel(response)
    self.updateUI(model)
}
```

**Not Recommended:**
```swift
// освобождение self может произойти между обновлением модели и обновлением пользовательского интерфейса, если обновление модели происходит не в главном потоке
resource.request().onComplete { [weak self] response in
    let model = self?.updateModel(response)
    self?.updateUI(model)
}
```

## Access Control

Full access control annotations in tutorials can be distracting from the main topic and are generally not required. However, using private and fileprivate in a targeted manner adds clarity and promotes encapsulation. It is better to use private over fileprivate whenever possible. The use of extensions may require the use of fileprivate.

Explicitly use open, public, and internal when you need a full specification of access control.

Use access control as the primary specifier for properties. Exceptions are only static or attributes such as @IBAction, @IBOutlet, @discardableResult, and @IBInspectable.

**Recommended:**
```swift
private let message = "Great Scott!"

class TimeMachine {
    fileprivate dynamic lazy var fluxCapacitor = FluxCapacitor()
}
```

**Not Recommended:**
```swift
fileprivate let message = "Great Scott!"

class TimeMachine {
    lazy dynamic fileprivate var fluxCapacitor = FluxCapacitor()
}
```

## Control Flow

It is preferable to use the for-in style for the for loop rather than the while-condition-increment style.

**Recommended:**
```swift
for _ in 0 ..< 3 {
    print("Hello three times")
}

for (index, person) in participants.enumerated() {
    print("\(person) is at position #\(index)")
}

for index in stride(from: 0, to: items.count, by: 2) {
     print(index)
}

for index in (0 ... 3).reversed() {
     print(index)
}
```

**Not Recommended:**
```swift
var i = 0
while i < 3 {
    print("Hello three times")
    i += 1
}

var i = 0
while i < participants.count {
    let person = participants[i]
    print("\(person) is at position #\(i)")
    i += 1
}
```

## The Golden Path

When writing conditional code, the left side of the code should be the so-called "golden" path. In other words, avoid adding unnecessary if statements by replacing them with multiple return statements. For this purpose, the guard statement was created.

**Recommended:**
```swift
func computeFFT(context: Context?, inputData: InputData?) throws -> Frequencies {
    guard let context = context else { throw FFTError.noContext }
    guard let inputData = inputData else { throw FFTError.noInputData }
    // используем context и inputData для подсчета frequencies
    return frequencies
}
```

**Not Recommended:**
```swift
func computeFFT(context: Context?, inputData: InputData?) throws -> Frequencies {
    if let context = context {
        if let inputData = inputData {
            // используем context и inputData для подсчета frequencies
            return frequencies
        } else {
            throw FFTError.noInputData
        }
    } else {
        throw FFTError.noContext
    }
}
```

## Using guard

When multiple conditional expressions are involved, add a line break before each operand. Complex conditions are better off in separate functions.

**Recommended:**
```swift
if 
    isTrue(),
    isFalse(),
    tax > MinTax {
    // ...
}
```

**Not Recommended:**
```swift
if a > b && b > c && tax > MinTax && currentValue > MaxValue {
    // ...
}
```

When multiple options are unwrapped through guard or if let, the nesting is significantly reduced. For example:

**Recommended:**
```swift
guard
    let number1,
    let number2,
    let number3
else { 
    clean()
    fatalError("impossible") 
}
//...
```

Or you can use the one-liner else option if there is only one statement inside:

```swift
guard 
    let number1, 
    let number2
else { return }
//...
```

or one-liner with one `let` operation

```swift
guard let number1 = number1 else { return }
```

**Not Recommended:**
```swift
if let number1 = number1 {
    if let number2 = number2 {
        if let number3 = number3 {
            //...
        } else {
            fatalError("impossible")
        }
    } else {
        fatalError("impossible")
    }
} else {
    fatalError("impossible")
}
```

### Handling guard

guard statements are required to indicate an exit in some way. Typically, this should be a simple one-liner statement such as return, throw, break, continue, or fatalError(). Large code blocks should be avoided. If there are multiple exit points that require cleanup code, it is recommended to use defer to avoid duplicating cleanup code.

## Semicolons

Swift does not require semicolons after each statement in your code. They are only necessary if you want to combine multiple statements on a single line. However, it is highly discouraged to do so.

**Recommended:**
```swift
let swift = "not a scripting language"
```

**Not Recommended:**
```swift
let swift = "not a scripting language";
```

## Parentheses

Parentheses around expressions are not required and should be omitted.

**Recommended:**
```swift
if name == "Hello" {
    print("World")
}
```

**Not Recommended:**
```swift
if (name == "Hello") {
    print("World")
}
```

In larger expressions, optional parentheses can sometimes make the code more readable.

**Рекомендуется:**
```swift
let isPlayerMarked = (player == current ? "X" : "O") && play
```

## Links

* [The Swift API Design Guidelines](https://swift.org/documentation/api-design-guidelines/)
* [The Swift Programming Language](https://developer.apple.com/library/prerelease/ios/documentation/swift/conceptual/swift_programming_language/index.html)
* [Using Swift with Cocoa and Objective-C](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/BuildingCocoaApps/index.html)
* [Swift Standard Library Reference](https://developer.apple.com/library/prerelease/ios/documentation/General/Reference/SwiftStandardLibraryReference/index.html)
