# Awesome Swift Property Wrappers

Examples of cool things you can do with Swift's Property Wrappers.

### User Defaults
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
- Clamped
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
