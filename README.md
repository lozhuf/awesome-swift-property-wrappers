# Awesome Swift Property Wrappers

Examples of cool things you can do with Swift's Property Wrappers.

> Note: In some examples of Property Wrappers the `initialValue` property is implicit. As of Xcode Beta 2 this is not the case, and initialValue must be explicitly specified.

### @Clamped

```swift
struct Foo {
    @Clamped(initialValue: 5, range: 0...10)
    static var bar: Int
    @Clamped(initialValue: 5, range: 8...)
    static var baz: Int
    @Clamped(initialValue: 5, range: ...4)
    static var bosh: Int
    @Clamped(initialValue: 5, min: 6)
    static var blob: Double
}

Foo.bar // 5
Foo.bar = 15
Foo.bar // 10
Foo.baz // 8
Foo.bosh // 4
Foo.blob // 6.0
```

<details>
<summary>
    🚀 Implementation
</summary>

```swift
@propertyWrapper
struct Clamped<Value: Comparable> {
    let min: Value?
    let max: Value?
    var actualValue: Value

    fileprivate init(initialValue: Value, minVal: Value?, maxVal: Value?) {
        self.actualValue = initialValue
        self.min = minVal
        self.max = maxVal
    }
    
    var value: Value {
        get {
            var val = self.actualValue
            
            if let min = self.min {
                val = Swift.max(val, min)
            }
            if let max = self.max {
                val = Swift.min(val, max)
            }
            return val
        }
        set {
            self.actualValue = newValue
        }
    }
}

extension Clamped {
    init(initialValue: Value, min: Value, max: Value) {
        self.init(initialValue: initialValue, minVal: min, maxVal: max)
    }
    init(initialValue: Value, min: Value) {
        self.init(initialValue: initialValue, minVal: min, maxVal: nil)
    }
    init(initialValue: Value, max: Value) {
        self.init(initialValue: initialValue, minVal: nil, maxVal: max)
    }
    
    init(initialValue: Value, range: ClosedRange<Value>) {
        self.init(initialValue: initialValue, min: range.lowerBound, max: range.upperBound)
    }
    init(initialValue: Value, range: PartialRangeFrom<Value>) {
        self.init(initialValue: initialValue, min: range.lowerBound)
    }
    init(initialValue: Value, range: PartialRangeThrough<Value>) {
        self.init(initialValue: initialValue, max: range.upperBound)
    }
}
```
</details>

### @UnitInterval
Uses the `@Clamped` propertyWrapper with a range of `0...1`.

```swift
struct Foo {
    @UnitInterval
    static var bar: Double = 0.5
}

Foo.bar // 0.5
Foo.bar = 2
Foo.bar // 1.0
```

<details>
<summary>
    🚀 Implementation
</summary>

```swift
@propertyWrapper
struct UnitInterval<Value: FloatingPoint> {
    @Clamped(initialValue: 0, range: 0...1)
    var value: Value
    
    init(initialValue: Value) {
        self.value = initialValue
    }
}
```
</details>

### @UserDefault

```swift
struct UDStore {
    @UserDefault("com.someApp.hasSeenIntro", defaultValue: false, defaultsStore: UserDefaults())
    static var hasSeenIntro: Bool
    
    @UserDefault("com.someApp.username", defaultValue: "Nathaniel Merriweather")
    static var username: String
}

UDStore.username // "Nathaniel Merriweather"
UDStore.username = "Dan"
UDStore.username // "Dan"
UDStore.$username.reset()
UDStore.username // "Nathaniel Merriweather"
```

<details>
<summary>
    🚀 Implementation
</summary>

```swift
@propertyWrapper
struct UserDefault<T> {
    let key: String
    let defaultValue: T
    let defaultsStore: UserDefaults
    
    init(_ key: String, defaultValue: T, defaultsStore: UserDefaults = .standard) {
        self.key = key
        self.defaultValue = defaultValue
        self.defaultsStore = defaultsStore
    }
    
    var value: T {
        get {
            return self.defaultsStore.object(forKey: key) as? T ?? defaultValue
        }
        set {
            self.defaultsStore.set(newValue, forKey: key)
        }
    }
    
    func reset() {
        self.defaultsStore.set(nil, forKey: key)
    }
}
```
</details>


## Possible ideas

To be fleshed out...

- https://github.com/DougGregor/swift-evolution/blob/property-wrappers/proposals/0258-property-wrappers.md
- Command line option
- Debounced/throttled
- Property Validation (https://medium.com/better-programming/swift-property-delegates-powerful-new-annotations-attributes-system-2e3968b29624)
- Localized
- Notification Trigger
- Aspect Ratio
- UIControl Action callbacks
- https://nshipster.com/propertywrapper/
    - A @Positive / @NonNegative property wrapper that provides the unsigned guarantees to signed integer types.
    - A @NonZero property wrapper that ensures that a number value is either greater than or less than 0.
    - @Validated or @Whitelisted / @Blacklisted property wrappers that restrict which values can be assigned.
    - An @Audited property wrapper that logs each time a property is read or written to.
    - A @Decaying property wrapper that divides a set number value each time the value is read.
    - Defining @CompatibilityEquivalence such that wrapped String properties with the values "①" and "1" are considered equal.
    - A @Approximate property wrapper to refine equality semantics for floating-point types (See also SE-0259)
    - A @Ranked property wrapper that takes a function that defines strict ordering for, say, enumerated values; this could allow, for example, the playing card rank .ace to be treated either low or high in different contexts.
    - A @Transformed property wrapper that applies ICU transforms to incoming string values.
    - A @Normalized property wrapper that allows a String property to customize its normalization form.
    - A @Quantized / @Rounded / @Truncated property that quantizes values to a particular degree (e.g. “round up to nearest ½”), but internally tracks precise intermediate values to prevent cascading rounding errors.
