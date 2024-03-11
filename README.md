
# Tone Framework


A Swift package for working with AudioKit and allows to perceive and filter tones (frequencies) 

## Requirements

- Swift 5.3+
- IOS 13+

## Usage

* * *

```swift
import ToneListen

// Instance tone framework by passing client ID
let toneFramework = ToneFramework(clientID: {{ Your client ID }})

//Start listen service:
toneFramework.start()

//To stop service:
toneFramework.stop()

//If you need to be notified when a valid tone was caught, you will need to subscribe to 'notificationName' from NotificationHandler:

// By publishing this property we can ensure that our subscriber (notificationName) will be
// re-rendered when the property changes (i.e. whenever there's a new notification)
@Published var newNotification: String? {
    didSet {
        guard notifiedValue != nil else { return }            
    }
}

notificationSub = NotificationCenter.default.publisher(for: NotificationsHandler.notificationName)
            .map { notification in notification.object as? String }   // Transform the notification into a simple string
            .assign(to: \notifiedValue, on: self)  // Assign the msg to a property using a keypath

// 'notifiedValue' saves an URL or PhoneNumber (string), depending what tone was caught. 
```
## NOTE:

You need to add "Background modes" capabilites to your project to allow using microphone and location services in background.

Also you need to add "frequencies_table.json" file to your project.


## Installation

### Swift Package Manager

Add the ToneLibrary package to your target dependencies in `Package.swift`:

```swift
import PackageDescription

let package = Package(
  name: "YourProject",
  dependencies: [
    .package(
        url: "https://github.com/Anilkumar18/ToneLibrary-iOS.git",
        from: "TonelibraryV2"
    ),
  ]
)
```

Or you can add the ToneLibrary package to your PROJECT package dependencies by following the steps below,

### 1. Click on (+) at the left bottom.
<img width="981" alt="Screenshot 2024-03-06 at 1 42 13 PM" src="https://github.com/Anilkumar18/ToneLibrary-iOS/assets/32846534/6c6bbfd3-8600-48ba-9f3e-749271b42e72">

### 2. Add the package url "https://github.com/Anilkumar18/ToneLibrary-iOS.git" into the package URL.

### 3. Select the "Exact version" - 8.0.3.
<img width="1075" alt="Screenshot 2024-03-06 at 2 10 19 PM" src="https://github.com/Anilkumar18/ToneLibrary-iOS/assets/32846534/edcfb9be-f4d0-42e9-8a56-b4318170da3f">

### 4. Click on Add package button.
<img width="650" alt="Screenshot 2024-03-06 at 2 12 52 PM" src="https://github.com/Anilkumar18/ToneLibrary-iOS/assets/32846534/472102ac-2a1e-47d4-9668-acd6a15ddc88">

### 5. The package now gets installed to your project.
