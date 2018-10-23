# userdefaults-set-nil-example
Example demonstrating the behavior of UserDefaults.set(nil, forKey:) in iOS 10.

The behavior of `UserDefaults.standard.set(nil, forKey: "myKey")` differs between iOS 10 and iOS 11.

In iOS 11 onward, the following code returns nil:

```swift
// iOS 11+
UserDefaults.standard.set(nil, forKey: "myKey")
print(UserDefaults.standard.data(forKey: "myKey")) // nil
```

However, in iOS 10, the same code produces an encoded plist:

```swift
// iOS 10
UserDefaults.standard.set(nil, forKey: "myKey")
if let data = UserDefaults.standard.data(forKey: "myKey") {
    let propertyList = try! PropertyListSerialization.propertyList(from: data, options: [], format: nil)
    print(propertyList)
}
```
```
{
    "$archiver" = NSKeyedArchiver;
    "$objects" =     (
        "$null"
    );
    "$top" =     {
        root = "<CFKeyedArchiverUID 0x7887b620 [0xe926e8]>{value = 0}";
    };
    "$version" = 100000;
}
```

This repo contains an example app project which demonstrates this behavior. To demonstrate, build and run the project on iOS 10 and 11 simulators and verify that the printed output is different.
