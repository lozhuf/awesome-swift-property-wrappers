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


### Command line option

### Debounced/throttled

### Property Validation
https://medium.com/better-programming/swift-property-delegates-powerful-new-annotations-attributes-system-2e3968b29624

### Localized
### Notification Trigger
### Aspect Ratio
### Clamped
