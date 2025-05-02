# SanarKit - iOS

SanarKit - is a Swift framework designed to seamlessly integrate Sanar services into your iOS application. It simplifies the process of connecting to Sanar's services, managing user sessions, and handling the complete booking and post-booking flows, including appointment and provider communication.

### SanarKit will support the following implementation

- ### [connect](https://github.com/MarenTech/SanarKit?tab=readme-ov-file#connect-1)
- ### [disconnect](https://github.com/MarenTech/SanarKit?tab=readme-ov-file#disconnect-1)
- ### [ServiceView](https://github.com/MarenTech/SanarKit?tab=readme-ov-file#serviceview-1)
- ### [InstantConsultationView](https://github.com/MarenTech/SanarKit?tab=readme-ov-file#instantconsultationview-1)
- ### [BookingListView](https://github.com/MarenTech/SanarKit?tab=readme-ov-file#bookinglistview-1)
- ### [ConsultationView](https://github.com/MarenTech/SanarKit?tab=readme-ov-file#consultationview-1)

## Installation Steps

To enable camera usage and microphone usage you will need to add the following entries to your Info.plist file:

```
<key>NSCameraUsageDescription</key>
<string>Your message to user when the camera is accessed for the first time</string>
<key>NSMicrophoneUsageDescription</key>
<string>Your message to user when the microphone is accessed for the first time</string>
```

## Import and Usage of SanarKit inside iOS Project file

```swift
Import SanarKit
```

## SanarKit Modules

# connect
The SanarKit SDK provides a seamless way to integrate authentication and connection services into your iOS applications. One of the key methods in the SDK is SKManager.connect, which allows your application to authenticate with SanarServices. To effectively integrate authentication and connection services into your iOS application using the SanarKit SDK, you should ensure the following:

#### - Initialization: Before invoking any Sanar flows or making use of other SDK features, you must initialize the SDK. This typically involves setting up required configurations and ensuring that your application is properly set up to communicate with SanarServices.
#### - Calling SKManager.connect: The SKManager.connect method should be called as an essential step to authenticate with SanarServices. This method establishes a connection and handles authentication, which is crucial for accessing any subsequent SDK functionalities or services.

### Example usage

```swift
SKManager.connect(
    cid: "<sanar-client-token>",
    bundleId: "<app-bundle-id>",
    clientData: ClientData,
    lang: "ar" // en
)
```

### Parameters : 
Key | Type | Description
:--- | :--- | :--- 
`cid` | String | Client ID provided by Sanar
`bundleId` | String | Bundle identifier of your app
`clientData` | Dictionary | [clientData](#clientdata-) contains the user or client data that needs to be sent to the SanarService to create the User Session
`lang` |  String | Optional |  the language parameter to switch between Arabic and English, default value will be English

#### clientData :
Property | Description
:--- | :---
first_name : string | First name
last_name : string  | Last name
dob : string  | Date of birth (`yyyy-mm-dd`)
gender : string  | Gender `M` | `F`
nationality : string | Nationality of User ex : Saudi Arabia.
document_id : string | Document id Default `1`
mid : string | Member id 
document_type: number | Document Type
phone_code : string  | Phone code ex : `966`
phone_no : string | Phone Number
marital_status : string | Marital status `0` : `Unmarried`, `1` : `Married`

#### Example usage with `clientData` : 

```swift
let clientData: [String: Any] = [
    "first_name": "John",
    "last_name": "Doe",
    "dob": "1994-08-13",
    "gender": "M",
    "nationality": "Saudi Arabia",
    "document_id": "2469433220",
    "mid": "MG2",
    "document_type": 1,
    "phone_code": "91",
    "phone_no": "81794771111",
    "maritalStatus": "0"
]

SKManager.connect(
    cid: "<client-id>",
    bundleId: "<com.example.demo>",
    requestBody: clientData
)
```

# disconnect
The SanarKit SDK also provides a method to disconnect from the SanarKit service, which is useful for terminating an existing connection when it's no longer needed.

```swift
SKManager.disconnect()
```

Best Practices: It's good practice to call `disconnect` when your app is about to be suspended, or when the user logs out or navigates away from sections of the app that require a connection to the SanarKit service.

# ServiceView
The SanarKit SDK includes a `ServiceView` that allows your application to navigate directly to the Sanar Booking flow. This flow handles the complete booking process with in the SDK, making it easy to integrate without requiring extensive additional development including UI interface.

```swift
SanarKit.ServiceView(isNavigationActive: $isService)
```

## Description : 
The `ServiceView` component in the SanarKit SDK is designed to facilitate the entire Booking process through a pre-built UI flow. By simply navigating to `ServiceView`, your application can leverage the SDK's capabilities to manage the booking flow without any extra setup or customization.

### Method Parameters : 
`isNavigationActive` (Binding<Bool>):

#### Method Parameter Description : 
A binding boolean that controls the navigation state to the `ServiceView`. When set to true, the app will navigate to the `ServiceView` and initiate the booking flow.

Example: `$isService`

#### Example Usage : 

```swift
import SwiftUI
import SanarKit

struct ContentView: View {
    @State private var isService = false
    
    var body: some View {
        NavigationStack {
            Button {
                isService = true
            } label: {
                Text("Book Service")
                    .foregroundColor(.blue)
            }
            .navigationDestination(isPresented: $isService) {
                SanarKit.ServiceView(isNavigationActive: $isService)
            }
        }
    }
}
```

#### How it works :
- Navigation: The NavigationLink uses the `isService` binding to determine whether to navigate to `ServiceView`. When `isService` becomes true, the app navigates to the `SanarKit.ServiceView`.
- Booking Flow: The `ServiceView` then manages the entire booking flow, providing a seamless user experience.

# InstantConsultationView
The SanarKit SDK provides an `InstantConsultationView` that enables your application to navigate directly to the Instant Teleconsultation flow. This view handles the complete booking and consultation process within the SDK. It includes:
	•	Instant appointment booking
	•	Chat with the provider once the appointment is confirmed
	•	Teleconsultation session handling

This simplifies integration and allows you to offer a seamless virtual consultation experience within your app.

```swift
SanarKit.InstantConsultationView(isNavigationActive: $isInstantService)
```

## Description : 
The `InstantConsultationView` component in the SanarKit SDK is designed to facilitate the entire Instant Tele Consultation process through a pre-built UI flow. By simply navigating to `InstantConsultationView`, your application can leverage the SDK's capabilities to manage the Instant Tele Consultation flow without any extra setup or customization.

### Method Parameters : 
`isNavigationActive` (Binding<Bool>):

#### Method Parameter Description : 
A binding boolean that controls the navigation state to the `InstantConsultationView`. When set to true, the app will navigate to the `InstantConsultationView` and initiate the Instant teleconsultation flow.

Example: `$isInstantService`

#### Example Usage : 

```swift
import SwiftUI
import SanarKit

struct ContentView: View {
    @State private var isInstantService = false
    
    var body: some View {
        NavigationStack {
            Button {
                isInstantService = true
            } label: {
                Text("Instant Teleconsultation")
                    .foregroundColor(.blue)
            }
            .navigationDestination(isPresented: $isInstantService) {
                SanarKit.InstantConsultationView(isNavigationActive: $isInstantService)
            }
        }
    }
}
```

#### How it works : 
- Navigation: The NavigationLink uses the `isInstantService` binding to determine whether to navigate to `InstantConsultationView`. When `isInstantService` becomes true, the app navigates to the `SanarKit.InstantConsultationView`.
- Booking Flow: The `InstantConsultationView` then manages the entire Tele consultation flow, providing a seamless user experience.

# BookingListView
The SanarKit SDK also provides a `BookingListView` that allows your application to navigate directly to the post-booking flow. This flow includes functionalities such as viewing appointment lists, appointment details, and chatting with the provider. By navigating to `BookingListView`, your app can seamlessly manage the post-booking process within the SDK.

```swift
SanarKit.BookingListView(isNavigationActive: $isBooking)
```

## Description : 

The `BookingListView` component in the SanarKit SDK is designed to handle the post-booking process. It provides a comprehensive view of appointments, including appointment lists, detailed information about each appointment, and an integrated chat and call feature for communication with the provider.

### Method Parameters :
`isNavigationActive` (Binding<Bool>):

#### Method Parameter Description : 
A binding boolean that controls the navigation state to the `BookingListView`. When set to true, the app will navigate to the `BookingListView` and manage the post-booking flow.

Example: `$isBooking`

#### Example Usage : 

```swift
import SwiftUI
import SanarKit

struct ContentView: View {
    @State private var isBooking = false

    var body: some View {
        NavigationStack {
            Button {
                isBooking = true
            } label: {
                Text("Appointments")
                    .foregroundColor(.blue)
            }
            .navigationDestination(isPresented: $isBooking) {
                SanarKit.BookingListView(isNavigationActive: $isBooking)
            }
        }
    }
}
```

#### How It Works
- Navigation: The NavigationLink uses the isBooking binding to determine whether to navigate to BookingListView. When isBooking becomes true, the app navigates to the SanarKit.BookingListView.
- Post-Booking Flow: The BookingListView then manages the entire post-booking flow, including displaying the appointment list, detailed information about each appointment, and providing a chat feature to communicate with the provider.

# ConsultationView
SanarKit provides the `ConsultationView` module to deeplink the app flow to the Consultation Page 

## Description : 
The `ConsultationView` module to deeplink the user directly to Consultation page when there is any callback notification from sanar and the app is in the background or killed state. Once the app is brought to the foreground by the callback notification, invoke this module by passing the required parameters to navigate to the Consultation page.

```swift
SanarKit.ConsultationView(
    consultationData: ["dId": "doctor_id", "aId": "appointment_id"]
)
```

### Parameters : 
	•	dId (String): The unique identifier of the doctor.
	•	aId (String): The appointment ID associated with the consultation.

### Example Usage : 

```swift
import SwiftUI
import SanarKit

struct ContentView: View {
    @State private var isConsultation = false
    
    var body: some View {
        NavigationStack {
            Button {
                isConsultation = true
            } label: {
                Text("Start Consultation")
                    .foregroundColor(.blue)
            }
            .navigationDestination(isPresented: $isConsultation) {
                SanarKit.ConsultationView(
                    consultationData: ["dId": "doctor_id", "aId": "appointment_id"]
                )
            }
        }
    }
}
```

#### How It Works
- The ConsultationView manages the doctor consultation process using the provided doctor and appointment IDs

# Demo Application
For more details and implementation examples, please check the example repository [Here](https://github.com/MarenTech/ExampleSanarKitSwift).


