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

### Command line option
…

### Debounce
…

### Property Validation
…
https://medium.com/better-programming/swift-property-delegates-powerful-new-annotations-attributes-system-2e3968b29624

### Localization
…
