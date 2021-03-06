---
title: "swift - enums"
date: 2020-02-01 10:47:30
---

# What is an Enumeration?

An **enumeration** are groups of values that are grouped by related types. For example, the days of the week can be represented by enums. It would look something like this:

    enum Day: Int {
        case Monday=1, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday
    }

## Raw Values

In the above example, we assigned the **raw value** for Monday to 1.

What is a **raw value**? A **raw value** is a string, character, or number (integer or floating-point) that can represent an enum case. The **raw value** can be accessed via the `rawValue` property:

    let monday = Day.Monday.rawValue 
    // returns 1

To convert a raw value to its respective enum case, use the initializer that takes a rawValue argument.

    let dayOfTheWeekNumber = 4
    let dayOfTheWeek = Day(rawValue: dayOfTheWeekNumber)
    print(dayOfTheWeek == Day.Thursday) // prints "true"

### Implicit Raw Values

The Swift compiler can infer the types of the raw values of enums. 

### String Enums

If the type of the enum is a `String` then each case is implicitly assigned a raw string value equal to the case's name. 

    enum Day: String {
        case Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday
    }

    print(Days.Friday.rawValue) // prints "Friday"

### Integer Enums

If the type of the enum is an `Integer` then each case is implicitly assigned a raw integer value equal starting a zero. 

    enum Day: Int {
        case Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday
    }

    print(Days.Friday.rawValue) // prints 4


It is also possible to define an enum with implicitly and explicitly defined raw values; all implicitly defined raw integer values will be one greater than the previous raw integer value. If no previous raw integer value exists, then the compiler will assign zero to the enum case.

    enum Day: Int {
        case Monday=0, Tuesday, Wednesday, Thursday=5, Friday, Saturday, Sunday
    }

    print(Days.Friday.rawValue) // prints 6


## Associated Values

**Associated Values** differ from **raw values** in that the enum cases can have varying types at the case level. They can add additional information to enum cases, can be any type, are tuples, and must be extracted to be used

In the example below, the `upc` enum case has an **associated value** of type `Int` and the `qrCode` case is of type `String`

    enum Barcode {
        case upc(Int, Int, Int, Int)
        case qrCode(String)
    }

    print(Barcode.upc(0, 0, 0, 0))



### Extracting Values via Switches

Use a `switch` to extract values from an enum. The switch cases must satisfy all values of the enum either explicitly or implicitly with `case default`


#### Explictly 

    let day = Days.Saturday

    switch day {
    case .Monday:
        print("Yikes beginning of the week")
    case .Tuesday:
        print("Going to the club on a Tuesday")
    case .Wednesday:
        print("HUMP DAY")
    case .Thursday:
        print("Getting Thirsty on a Thursday")
    case .Friday:
        print("FriYAY!!!!")
    case .Saturday:
        print("CATURDAY")
    case .Sunday:
        print("Sunday blues")
    }

    // prints "CATURDAY"

#### Implicitly

    let day = Days.Saturday

    switch day {
    case .Monday:
        print("Yikes beginning of the week")
    case .Tuesday:
        print("Going to the club on a Tuesday")
    case .Wednesday:
        print("HUMP DAY")
    case .Thursday:
        print("Getting Thirsty on a Thursday")
    default:
        print("WEEKEND")
    }

    // prints "WEEKEND"

### Extracting Values via If Case

To extract an associated value for a single case, an `if case` statement can be used.

    enum Barcode {
        case upc(Int, Int, Int, Int)
        case qrCode(String)
    }

    let barcode = Barcode.upc(1,2,3,4)

    if case Barcode.upc(let num1, var num2, var num3, _) = barcode {
        num2 = 10
        print(num2) // prints "10"
    }

    // conditionally extract an associated value

    if case Barcode.upc(let num1, var num2, var num3, _) = barcode, num2 == 2 {
        num2 = 10
        print(num2) // prints "10"
    }