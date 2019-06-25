# Awesome Swift Property Wrappers

Examples of cool things you can do with Swift's Property Wrappers.

### User Defaults
```swift
struct MyUserDefaultsStore {
    @UserDefault("com.someApp.hasSeenIntro", defaultValue: false)
    static var hasSeenIntro: Bool

    @UserDefault("com.someApp.username", defaultValue: "Nathaniel Merriweather")
    static var username: String
}
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

    init(_ key: String, defaultValue: T) {
        self.key = key
        self.defaultValue = defaultValue
    }

    var value: T {
        get {
            return UserDefaults.standard.object(forKey: key) as? T ?? defaultValue
        }
        set {
            UserDefaults.standard.set(newValue, forKey: key)
        }
    }
}
```
</details>


### Command line option

### Debounced/throttled

### Property Validation
https://medium.com/better-programming/swift-property-delegates-powerful-new-annotations-attributes-system-2e3968b29624

### Localized
### Notification Trigger
### Aspect Ratio
### Clamped
