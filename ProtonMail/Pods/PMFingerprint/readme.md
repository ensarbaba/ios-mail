# Fingerprint

Collect fingerprint during user signup so that the anti-abuse team can analytic.
For more detail about collected fingerprint please read [iOS fingerprints](https://confluence.protontech.ch/pages/viewpage.action?spaceKey=PRODUCT&title=iOS+fingerprints)

## Dependency
* `UIKit`
* `Foundation`
* `CoreTelephony`: to collect cellular information

## Install
### CocoaPods
```
pod 'PMFingerprint', :git => 'https://gitlab.protontech.ch/apple/shared/pmfingerprint.git', :tag => '{TAG}'
```
### Swift Package Manager
1. Using Xcode 11 go to File > Swift Packages > Add Package Dependency
2. Paste the project URL: `https://gitlab.protontech.ch/apple/shared/pmfingerprint`
3. Click on next and select the project target

## How to use
### Initialize
There are two ways to initialize `PMFingerprint`
1. Singleton
```swift
/// Get shared instance, thread safe
public static func shared() -> PMFingerprint

/// Release shared instance if you don't need it anymore.
public static func release()
```
2. Common initialize
```swift
let fingerprint = PMFingerprint()
```

### Public functions
1. Reset collected fingerprint data
```swift
public func reset()
```
2. Export collected fingerprint data
```swift
public func export() -> PMFingerprint.Fingerprint
```
3. Start to observe given textfield so that we can monitor copy/paste/characters changed...etc

If you call this function with the same type (or same textField) twice.
The data of the first called will be overridden by the second called.
```swift
public func observeTextField(_ textField: UITextField, type: TextFieldType, ignoreDelegate: Bool = false) throws
```
support textfield types are below.     
we collect different data depends on the type.      
```swift
public enum TextFieldType {
    /// TextField for username
    case username
    /// TextField for password
    case password
    /// TextField for password confirm
    case confirm
    /// TextField for recovery mail
    case recovery
    /// TextField for verification
    case verification
}
```
Note, this library supports these types.      
but it doesn't mean every type will get the same action monitor.

4. Record username that checks availability
Please use this function every time when user checks availability.
```swift
public func appendCheckedUsername(_ username: String)
```
5. Declare user start request verification (email/ sms/ captcha) so that timer starts.
```swift
public func requestVerify()
```
6. Count verification time
```swift
public func verificationFinsih() throws
```
