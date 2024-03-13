
# Tone Framework Guide - iOS

## Introduction:-

**Tone Framework** is a library available for iOS applications, developed using Swift and leveraging **AudioKit** to analyze audio input through the iPhone's microphone. It is designed to detect specific frequencies of tones, enabling the app to display information or perform actions when these tones are identified.

## Requirements:-

- Requires client key : Obtain the client key from Tone Support.
- Requires Swift version 5.3 or above.
- Requires iOS version 13.0 or above.

## Permissions:-

You need to add “**Background modes**” capabilities to your project to allow using **microphone** and **location** service in the background.

Ensure you have the following entries in your ‘**Info.plist**’ file,

- ‘**NSMicrophoneUsageDescription**’: A string describing the usage of requesting access to the microphone.
- ‘**NSLocationWhenInUseUsageDescription**’: A string describing the usage of requesting location access. 

Add the above permissions on your ‘**Info.plist**’ file like the following below,

```
    <key>NSMicrophoneUsageDescription</key>
    <string>Our app requires microphone access to record audio.</string>
    <key>NSLocationWhenInUseUsageDescription</key>
    <string>Our app requires location access to provide location-based services.</string>

```

## Installation:-

Use Swift package manager to integrate our “**ToneFramework**” package into your iOS application.

Add the “**ToneFramework**” package into your **PROJECT package dependencies** by following the steps below.

1. Click on the (+) icon at the left bottom.

![image2](https://github.com/The-TONE-Knows-Inc/ToneFramework/assets/156199216/74d67254-ae20-4085-abeb-22ff6f015c9a)

2. Add the below url into the package URL search box.

“https://github.com/The-TONE-Knows-Inc/ToneFramework.git”

3. Select the “**Up to Next Major Version**” - **1.0.0** from the **Dependency Rule** and select your project from **Add to Project** dropdown.
   
![image3](https://github.com/The-TONE-Knows-Inc/ToneFramework/assets/156199216/92076d95-8c16-4db0-af48-e913ca306fd3)

4. Click on the “**Add package**” button from the below shown dialog box.
   
<img width="820" alt="image1" src="https://github.com/The-TONE-Knows-Inc/ToneFramework/assets/156199216/20f7f7ce-eaff-4f78-8841-2ad6374ea4ee">

After these steps, the Tone Framework package will be installed in your project.

## Integration:-

Incorporating the Tone Framework into your application involves the following steps:

1. **Import the Tone Framework:** Include the framework in your file with the following header file,

```swift
import ToneListen
```

2. **Import the AVFoundation and CoreLocation:** Include the header files for microphone and location access,

```swift
 import AVFoundation
 import CoreLocation
```

3. **Add Delegate for Location permission:** Add the delegate property ‘CLLocationManagerDelegate’ on the class declaration for location access callbacks,

```swift
class ViewController: UIViewController, CLLocationManagerDelegate  {
```
add ‘self’ to that concern class,

```swift
locationManager = CLLocationManager()
locationManager.delegate = self
```
and request location permission.

```swift
// Request location permission
locationManager.requestWhenInUseAuthorization()
```

4. **Mic permission handling:** This code snippet demonstrates how to request and handle microphone access permissions in an iOS application using the ‘AVAudioSession’ class. Proper handling of microphone permissions is essential for apps that record audio or require audio input functionality.

```swift
AVAudioSession.sharedInstance().requestRecordPermission { [weak self] granted in
    if granted {
        print("Microphone access granted")
        toneFramework.setClientId(clientID: "900005")
        toneFramework.start()
    } else {
        print("Microphone access denied")
    }
}
```

5. **Initialize:** Utilize the shared instance for framework functionalities,
   
```swift
let toneFramework = ToneFramework.shared
```

6. **Configuration:** Input the client key obtained from Tone Support,

```swift
toneFramework.setClientId(clientID: { YOUR_CLIENT_ID })
```

7. **Start Tone Detection:** Activate tone detection service using the start function from the Tone Framework,

```swift
toneFramework.start()
```

8. **Stop Tone Detection:** Deactivate the tone detection service by utilizing the stop function from the Tone Framework,

```swift
toneFramework.stop()
```

9. **Action Handling:** Create a mapping between detected tones and corresponding actions. This could range from displaying specific content, triggering navigational changes within the app, initiating multimedia playback, or even activating external services or devices.

    Consider providing immediate visual or auditory feedback to the user upon tone detection to enhance the interactive experience.

## Implement Tone Detection Callback:-

To harness the full potential of the Tone Framework within your application, it's essential to integrate callback mechanisms that the framework provides. These callbacks or delegate methods serve as conduits for receiving real-time notifications about tone detections, enabling your application to respond dynamically to auditory events.

    1. **Callback Integration:** Incorporate the Tone Framework's callback system into your application to monitor and react to tone detection events. When a specific tone is identified by the framework, a notification is dispatched, which your application can listen for and subsequently process.

    The provided code snippet demonstrates an effective way to listen for and handle tone detection notifications using Combine in a SwiftUI environment:

```swift
    import Combine

    var notificationName = NotificationsHandler.notificationName
    var notificationSub: AnyCancellable?

    var newNotification: String? {
        didSet {
            guard let newNotification = newNotification else { return }
            // Implement your custom logic to respond to the detected tone
            print("Received new notification: \(newNotification)")
        }
    }

    override func viewDidLoad() {
        super.viewDidLoad()
        // Set up a subscriber to listen for tone detection notifications
        notificationSub = NotificationCenter.default.publisher(for: notificationName)
            .receive(on: DispatchQueue.main)
            .map { $0.object as? String }
            .sink { [weak self] notificationString in
                self?.newNotification = notificationString
            }
    }
```

    In this setup, your application subscribes to notifications posted by the Tone Framework. When a tone is detected, the framework emits a notification carrying details about the event, which is then captured by the subscriber in your application. Within the ‘**newNotification**’ property's **didSet** observer, you can define custom logic to process the detected tone, such as updating the UI or triggering specific actions within the app.

If you prefer not to use Combine, you have other options to handle callbacks and notifications in Swift:

1. **Notification Center:** To receive and act upon the data from the framework, your app needs to listen for specific notifications. Subscribe to these notifications in the relevant view controllers or app components:

```swift
NotificationCenter.default.addObserver(self, selector: #selector(handleFrameworkNotification(_:)), name: NotificationsHandler.responseObjectNotificationName, object: nil)
```

2. **Handling Framework Notifications:** When your app receives a notification, extract the **actionType** and **actionUrl** to determine the next steps. Here's a simplified example:

```swift
@objc func handleFrameworkNotification(_ notification: Notification) {
    if let content = notification.object as? [String: Any], let actionType = content["actionType"] as? String, let actionUrl = content["actionUrl"] as? String {
        RemoteService.handleContent(actionType: actionType, actionUrl: actionUrl)
    }
}
```

These integration steps will be equipped to receive and act upon notifications from the custom framework.


By following these guidelines, you can successfully incorporate the Tone Framework into your iOS application, enhancing its interactivity and responsiveness to auditory cues.

* * *

