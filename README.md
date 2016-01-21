# Our Swift Style Guide

Swift language style guide &amp; coding conventions followed by xmartlabs.com team.

The goals of this guide are:

1. Be syntactically consistent on how we write swift code no matter who does it.
2. Write readable and maintainable code.
3. Focus on real software problems rather than on how we style the code.

### Naming

Use descriptive names with camel case for classes, structs, enums, methods, properties, variables, constants, etc.  Only Classes, Enums, Structs names should be capitalized. Don't care about name length.

**prefered**

```swift

let maxSpeed = 150.0
var currentSpeed = 46.5
var numberOfWheels = 3

func accelerateVehicleTo(speed: Double) {
  currentSpeed = speed
}
```

**not prefered**

```swift
let NUMBER_OF_WHEELS = 3
let MAX_SPEED = 150.0

var mCurrentSpeed = 46.5

func accelerate_vehicleTo(speed: Double) {
  mCurrentSpeed = speed
}
```

### Line-wrapping

#### Where to break

The prime directive of line-wrapping is: prefer to break at a higher syntactic level. Also:

1. When a line is broken at a non-assignment operator the break comes before the symbol.
    * This also applies to the following "operator-like" symbols: the dot separator (.).
2. When a line is broken at an assignment operator the break typically comes after the symbol, but either way is acceptable.
3. A method or constructor name stays attached to the open parenthesis (() that follows it.
4. A comma (,) stays attached to the token that precedes it.

#### Indent continuation lines at least +4 spaces

When line-wrapping, each line after the first (each continuation line) is indented at least +4 from the original line.

**preferred**

```swift
    func ownersOfCarsWithoutFuel(cars: [Car]) -> [Owner] {
      return cars.filter { $0.fuelLevel == 0 }
                 .map { $0.owner }
    }
```

**not preferred**

```swift
    func ownersOfCarsWithoutFuel(cars: [Car]) -> [Owner] {
      return cars.filter { $0.fuelLevel == 0 }
        .map { $0.owner }
    }
```

When there are multiple continuation lines, indentation may be varied beyond +4 as desired. In general, two continuation lines use the same indentation level if and only if they begin with syntactically parallel elements.

### Whitespace

* Tabs, not spaces.
* Always end a file with a newline.
* Never leave trailing whitespace.

#### Vertical Whitespace

A single blank line appears:

1. Between consecutive members (or initializers) of a class: fields, constructors, methods, nested classes, static initializers, instance initializers.
    * **Exception**: A blank line between two consecutive fields (having no other code between them) is optional. Such blank lines are used as needed to create logical groupings of fields.
2. Within method bodies, as needed to create logical groupings of statements.
3. Before the first member and after the last member of the class.
4. As required by other sections of this document.

Multiple consecutive blank lines are not permitted.

Next reserved words won't start in a new line: else, catch

#### Horizontal whitespace

Beyond where required by the language or other style rules, and apart from literals, comments and doc, a single ASCII space also appears in the following places only.

1. Separating any reserved word, such as if, from an open parenthesis (() that follows it on that line
2. Separating any reserved word, such as else or catch, from a closing curly brace (}) that precedes it on that line
3. Before any open curly brace ({):
4. On both sides of any binary or ternary operator. This also applies to the following "operator-like" symbols:
    * the colon (:) in an inline if
5. After (,:) or the closing parenthesis ()) of a cast
6. On both sides of the double slash (//) that begins an end-of-line comment. Here, multiple spaces are allowed, but not required.

A whitespace before colon (:) in a property declaration, class inheritance declaration and arguments labels declaration **must not** be added

**preferred**

```swift
class Car: Vehicle {

    let numberOfDoors: Int
    let maxSpeed: Float

    let brand: String?
    let model: String?

    var fuelLevel = 0
    var temperature = 0.0

    init(numberOfDoors: Int, maxSpeed: Float, brand: String?, model: String?) {
        super.init()
        self.numberOfDoors = numberOfDoors
        self.maxSpeed = maxSpeed
        self.brand = brand
        self.model = model
    }

    func fuelStatusDescription() -> String {
        if fuelLevel < 0.1 {
            return "\(Warning), not enough fuel!"
        } else {
            return "Enough fuel to travel."
        }
    }

    func temperatureStatusDescription() -> String {
      return temperature > 28 ? "To much hot, turn on air conditioner" : "Great temperature condition"
    }
}
```

**not preferred**

```swift
class Car : Vehicle {
    let numberOfDoors : Int

    let maxSpeed : Float

    let brand : String?

    let model : String?

    var fuelLevel = 0


    var temperature = 0.0

    init(numberOfDoors: Int, maxSpeed: Float, brand: String?, model: String?) {
        super.init()
        self.numberOfDoors = numberOfDoors
        self.maxSpeed = maxSpeed
        self.brand = brand
        self.model = model
    }

    func fuelStatusDescription() -> String {
        if fuelLevel < 0.1{
            return "\(Warning), not enough fuel!"
        }
        else{
            return "Enough fuel to travel."
        }
    }

    func temperatureStatusDescription() -> String {
      return temperature > 28 ? "To much hot, turn on air conditioner":"Great temperature condition"
    }
}
```

### Semicolon

* Do not use semicolon at the end of a line.

> End line semicolon is optional in swift.

**prefered**
```swift
let numberOfWheels = 4
```

**not prefered**
```swift
let numberOfWheels = 4;
```

### let vs var

Always use `let` if a variable value won't change.
It's clear for a developer that a `let` reference can not be changed which makes easy to read and understand a code. Whenever a `var` appears a developer expects that its value changes somewhere.

> Normally swift compiler complains if we use `var` and the variable does not change.

### self

Only use `self` when strictly necessary. For instance within closures or to disambiguate a symbol.

> Swift does not require `self` to access an instance property or to invoke its methods.

```swift
class Person {
  let name: String
  var skills = []

  init(name: String) {
    self.name = name // self needed to assign the parameter value to the property that has the same name.
  }

  func resetSkills() {
    skills = [] // Note that we don't use self here
  }
}
```

### Optionals

#### Avoid Force Unwrapping

Optional types should not be forced unwrapped.

> Trying to use ! to access a non-existent optional value triggers a runtime error.

Whenever possible prefer to use *Optional Binding* or *Optional Chaining*.

```swift
var vehicle = Vehicle?
```

**prefered**

```swift
vehicle?.accelerateVehicleTo(65.3)
```

**not prefered**

```swift
vehicle!.accelerateVehicleTo(65.3)
```

**prefered**

```swift
if let vehicle = vehicle {
  parkVehicle(vehicle)
}
```

**not prefered**

```swift
if vehicle != nil {
  parkVehicle(vehicle!)
}
```

#### Multiple optional Binding & where usage

```swift
var vehicleOne: Vehicle? = ...
var vehicleTwo: Vehicle? = ...
```

**prefered**

```swift
if let vehicleOne = vehicleOne, vehicleTwo = vehicleTwo where vehicleOne.currentSpeed > vehicleTwo.currentSpeed {
    print("\(vehicleOne) is faster than \(vehicleTwo)")
}
```

**not prefered**

```swift
if let vehicleOne = vehicleOne {
    if let vehicleTwo = vehicleTwo {
        if vehicleOne.currentSpeed > vehicleTwo.currentSpeed {
            print("\(vehicleOne) is faster than \(vehicleTwo)")        
        }
    }  
}
```

#### Nil Coalescing Operator

> The nil coalescing operator (a ?? b) unwraps an optional a if it contains a value, or returns a default value b if a is nil.
> It's shorthand for the code below:

> ```swift
> a != nil ? a! : b
> ```

> The nil coalescing operator provides a more elegant way to encapsulate this conditional checking and unwrapping in a concise and readable form.

**prefered**

```swift
public func textFieldShouldReturn(textField: UITextField) -> Bool {
    return formViewController()?.textInputShouldReturn(textField, cell: self) ?? true
}
```

**not prefered**

```swift
public func textFieldShouldReturn(textField: UITextField) -> Bool {
    guard let result = formViewController()?.textInputShouldReturn(textField, cell: self) else {
      return true
    }
    return result
}
```

**prefered**

```swift
func didSelect() {
  row.value = !(row.value ?? false)
}
```

**not prefered**

```swift
func didSelect() {
  row.value = row.value != nil ? !(row.value!) : true
}
```

**prefered**

```swift
switchControl?.on = row.value ?? false
```

**not prefered**
```swift
if let value = row.value {
    switchControl?.on = value
}
else {
    switchControl?.on = false
}
```

#### Avoid Implicit unwrapping Optionals

Prefer to use `?` over `!` if the property/variable value can be nil.

```swift
class BaseRow {
    var section: Section? // can be nil at any time, we should use ?
}
```

Only use `!` when strictly necessary. For instance if a property value can not be set up at initialization time but we can ensure it will be set up before we start using it.

> Explicit optionals are safer whereas Implicit unwrapping optionals will crash at runtime if the value is nil.

```swift
class BaseCell: UITableViewCell
    // Every BaseCell has a baseRow value that is set up by a external library right after BaseCell init is invoked.
    var baseRow: BaseRow!
}
```

### Access Control

By default an entity access control level is internal which means it can be used from within the same module.
Typically this access control modifier works and in this cases we should not explicit define a internal modifier.

> In Swift, no entity can be defined in terms of another entity that has a lower (more restrictive) access level.


### Control Flow

Whenever possible prefer to use `for-in` statements over traditional for c-style:

**prefered**

```swift
for index in 1..<5 {
  ...
}
```

**not prefered**

```swift
for var index = 0; index < 5; ++index {
  ...
}
```

**prefered**

```swift
var items = ["Item 1", "Item 2", "Item 3"]

for item in items().enumerate() {
    segmentedControl.insertSegmentWithTitle(item.element, atIndex: item.index, animated: false)
}
// or
for (index, item) in items().enumerate() {
    segmentedControl.insertSegmentWithTitle(item, atIndex: index, animated: false)
}
```

**not prefered**

```swift
for var index = 0; index < items.count; i++ {
  let item = items[index]
  segmentedControl.insertSegmentWithTitle(item, atIndex: index, animated: false)
}
```

#### Early Exit

Use `guard` if some condition must be true to continue the execution of a method. Check the condition as early as possible.

> Swift compiler checks that the code inside the guard statement transfers the control to exit the code block that the guard appears in.

**prefered**

```swift
func getOutPassenger(person: Person) {
    guard passenger.contains(person) && vehicle.driver != person else {
        return
    }
    ....
}
```

**not prefered**

```swift
func getOutPassenger(person: Person) {
    if !passenger.contains(person) || vehicle.driver == person else {
        return
    }
    ....
}
```

### Protocols

Whenever possible conform to a protocol through an extension. Use one extension per protocol conformance.

**prefered**

```swift
class XLViewController: UIViewController {
  ...
}

extension XLViewController: UITableViewDataSource {
  ...
}

extension XLViewController: UITableViewDelegate {
  ...
}
```

**not prefered**

```swift
class XLViewController: UIViewController, UITableViewDataSource, UITableViewDelegate {
  ...
}
```

> One limitation of conforming protocol through extension is that we can not override implementations neither invoke super implementation.

### Properties

Whenever possible instantiate the property inline.

**prefered**

```swift
var currentSpeed = 46.5
```

**not preferred**

```swift
var currentSpeed: Double

init() {
  self.currentSpeed = 46.5
}

```


Don't specify property type if it can be inferred.

**prefered**

```swift
var dismissViewControllerWhenDone = false
```

**not prefered**

```swift
var dismissViewControllerWhenDone: Bool  = false
```

Array property declaration:

**prefered**

```swift
var vehicles = [Vehicle]()
//or
var vehicles: [Vehicle]
```

**not prefered**

```swift
var vehicles = Array<Vehicle>()
//or
var vehicles: Array<Vehicle>
// or
var vehicles: Array<Vehicle> = []
```

#### Read only computed properties and subscripts

We use implicit getters if a computed property or subscript is read-only.

**prefered**
```swift
var hasEngine: Bool {
    return true
}

subscript(index: Int) -> Passenger {
    return passenger[index]
}
```

**not prefered**
```swift
var hasEngine: Bool {
    get {
        return true
    }
}

subscript(index: Int) -> Passenger {
    get {
        return passenger[index]
    }
}

```


#### Enumerations

Enumeration values should be begin with a capital letter.
One option per line.

**prefered**

```swift
public enum HeaderFooterType {
    case Header
    case Footer
}
```

**not prefered**

```swift
public enum HeaderFooterType {
    case Header, Footer
}
// or
public enum HeaderFooterType {
    case header
    case footer
}
```

##### Shorter dot syntax

Usually enum type can be inferred and in these cases we prefer the short dot syntax over long typed syntax.


**prefered**

```swift
var directionToHead = CompassPoint.West // same as var directionToHead: CompassPoint = .West
directionToHead = .East
..
headToDirection(.West)
```

**not prefered**

```swift
var directionToHead = CompassPoint.West
directionToHead = CompassPoint.East // here the type of directionToHead is already know so the CompassPoint type is unnecessary.
headtoDirection(CompassPoint.West)
```

##### Multiple cases in the same line

Whenever possible use multiple cases in the same line.

**prefered**
```swift
switch service {
case .ChangeEmail, .ChangeUserName:
  return .POST
}
```

**not prefered**

```swift
switch service {
case .ChangeEmail:
  return .POST
case .ChangeUserName:
  return .POST
}
```

Sometimes more than one option share the same switch case logic

#### Strings

String interpolation is prefered over string concatenation using +

**prefered**

```swift
let message = "\(multiplier) times 2.5 is \(Double(multiplier) * 2.5)"
```

**not prefered**
```swift
let message = String(multiplier) + " times 2.5 is " + String(Double(multiplier) * 2.5)
```

#### Error Handling

##### try? - Converting Errors to Optional Values

Use try? to write concise error handling code when you want to handle all errors in the same way.

> You use try? to handle an error by converting it to an optional value. If an error is thrown while evaluating the try? expression, the value of the expression is nil.

**prefered**

```swift
let x = try? someThrowingFunction() ?? []
```

**not prefered**

```swift
var x : [AnyObject]
do {
  x = try someThrowingFunction()
}
catch {
  x = []
}
```


--------------------------------------------

Some of the style and syntaxis conventions we follow are checked using [SwiftLint](https://github.com/realm/SwiftLint) realm project.

This guide may suffer changes due to swift language evolution.

We would love to hear your ideas! Feel free to contribute by creating a pull request!
