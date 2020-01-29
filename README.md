# Octus Aadhaar Offline SDK - iOS
![version](https://img.shields.io/badge/version-v1.0.0-blue)

Aadhaar Paperless Offline eKYC is a secure and shareable document which can be used by any Aadhaar holder for offline verification of identification. The Aadhaar Offline document can be obtained from the UIDAI website. This SDK provides a simple plugin to your mobile App which allows the user to seamlessly share their offline Aadhaar file with the service provider. 


There are two ways the SDK can be configured within your App. The first one is an in-app experience whereby the pre-built screens will allow the Aadhaar holder to enter the Aadhaar Number or VID, Captcha, OTP and four digit share code all within your App without redirecting to the UIDAI website. The Aadhaar Offline file once downloaded will be parsed in-memory and displayed in the App (data shared with the App as JSON data). The experience is seamless with the in-app option.


The second option is to redirect the user to the UIDIAI website and allow the user to follow the instructions provided by the website. The user can enter the Aadhaar Number or VID, captcha, OTP and four digit share code in the website. Once the data is validated by UIDAI, a ZIP file (password protected using the share code) will be downloaded into the resident’s device. The user will now have to switch back to your App and select the file from the devices download folders which will parse the data and display them in the App (data shared with the App as JSON data). 


In both cases, the Aadhaar Offline file will be validated for its digital signature and the KYC data of The Aadhaar holder will be passed to the integrating App as JSON data.

You can find the release history at [Changelog](CHANGELOG.md)

# Table Of Content
- [Prerequisite](#prerequisite)
- [iOS SDK Requirements](#requirements)
- [Installation](#installation)
- [Usage example](#Usage-example)
- [Octus Result](#Octus-Result)
- [Octus Parameters](#octus-parameters)
- [Octus Error Codes](#octus-error-codes)
- [Help](#help)

## Prerequisite


You will need a valid license to use the Octus Aadhaar Offline SDK, which can be obtained by contacting `support@frslabs.com` 

Depending on the license - offline or online - you have opted for, the ping functionality to billing servers will be disabled or enabled. For instance, if you have opted for the offline SDK model, then there will be no server ping needed to our billing server to bill you. However, if you have chosen a transaction based pricing, then after each transaction, a ping request will be made to our billing server. This cannot be overrided by the App. A point to note is that if the ping transaction fails for any reason, the whole transaction will be void without any results from the SDK.


## iOS SDK Requirements

- iOS 10.0+
- Xcode 11.2


## Installation

#### CocoaPods
You can use [CocoaPods](http://cocoapods.org/) to install `aadhaarOffline` by adding it to your `Podfile`:

```ruby
platform :ios, '13.0'
source 'https://gitlab.com/frslabs-public/ios/aadhaaroffline.git'
use_frameworks!
pod 'aadhaarOffline'
pod 'Zip'
```

To get the full benefits import `aadhaarOffline` wherever you import UIKit

``` swift
import UIKit
import aadhaarOffline
```


## Usage example

####  File SDK 

```swift
import UIKit
import aadhaarOffline

let aadhaarXMLFileParser = OctusAadhaarOffline(licenceKey: LICENCE_KEY_OCTUS_AADHAR_OFFLINE_SDK, filePath: SELECTED_FILE_URL, password: PASSWORD)
 aadhaarXMLFileParser.run()
 aadhaarXMLFileParser.getOctusResult { (octusAadhaarResult, type) in
     switch type {
     case .success:
     print("OctusResult: ", octusAadhaarResult)
     case .failed(let fail):
     print("Fail: ",fail)
     case .error(let error):
         print("Error",error)
     }
 }
```
####  In-App SDK 

```swift
 import UIKit
 import aadhaarOffline
 
 let aadhaarInAppParser = OctusInappAadhaarOffline()
       aadhaarInAppParser.licenceKey = LICENCE_KEY_OCTUS_AADHAR_OFFLINE_SDK
       aadhaarInAppParser.octusBaseUrl = OCTUS_AADHAR_OFFLINE_API_BASE_URL
       aadhaarInAppParser.octuskeyId = OCTUS_AADHAR_OFFLINE_KEY_ID
       aadhaarInAppParser.octuskeySecret = OCTUS_AADHAR_OFFLINE_KEY_SEC
       aadhaarInAppParser.modalPresentationStyle = .fullScreen
       aadhaarInAppParser.callback = {
           (octusAadhaarResult) in
           print("OctusResult: ",octusAadhaarResult)
       }
       self.present(aadhaarInAppParser, animated:true, completion:nil)

``` 


## Octus AadhaarOffline Result

```swift

 let referenceId = octusAadhaarResult.referecnceId
 let uid = octusAadhaarResult.uid
 let timeStamp = octusAadhaarResult.timeStamp
 let name = octusAadhaarResult.name
 let careOf = octusAadhaarResult.co
 let dob = octusAadhaarResult.dob
 let gender = octusAadhaarResult.gender
 let house = octusAadhaarResult.house
 let street = octusAadhaarResult.street
 let lm = octusAadhaarResult.lm
 let loc = octusAadhaarResult.loc
 let vtc = octusAadhaarResult.vtc
 let po = octusAadhaarResult.po
 let subDist = octusAadhaarResult.subdist
 let dist = octusAadhaarResult.dist
 let state = octusAadhaarResult.state
 let country = octusAadhaarResult.country
 let pc = octusAadhaarResult.pc
 let yob = octusAadhaarResult.yob
 let facePhoto = octusAadhaarResult.photo
 
```

## Octus Error Codes

Error codes and their meaning are tabulated below

| Code          | Message              |
| ------------- | ------------------- |
| 1101  |  Licence Expired           |
| 1102  |  Licence Invalid            |
| 1103  | Invalid File URL            |
| 1104  | Incorrect Zip  File Password    |
| 1105  | Transaction Failed         |


## Octus Parameters

   ***(File SDK)***
   ```swift
   let aadhaarXMLFileParser = OctusAadhaarOffline(licenceKey: licenceKey, filePath: selectedFileUrl!, password: "share_code")
 ```
  - LICENCE_KEY_OCTUS_AADHAR_OFFLINE_SDK - Accept the octus aadhaar offline licence key as a String
  - SELECTED_FILE_URL - Selecting downloaded ZIP file url by following method
    
    Download link - https://resident.uidai.gov.in/offline-kyc
    
       ```swift
          let documentPicker = UIDocumentPickerViewController(documentTypes: ["public.zip-archive", "com.pkware.zip-archive"], in: .import)
            documentPicker.delegate = self
            documentPicker.modalPresentationStyle = .fullScreen
            present(documentPicker, animated: true, completion: nil)
            
            func documentPicker(_ controller: UIDocumentPickerViewController, didPickDocumentAt url: URL) {
                        self.SELECTED_FILE_URL = url as URL
                }

     ```
     - PASSWORD - Zip password as a String (Entered at a time of download xml from UIDAI website)
   
***(In-App SDK)***

- LICENCE_KEY_OCTUS_AADHAR_OFFLINE_SDK - Accept the octus aadhaar offline licence key as a String
- OCTUS_AADHAR_OFFLINE_API_BASE_URL - Accept aadhaar BASE URL as a String
- OCTUS_AADHAR_OFFLINE_KEY_ID - Accept aadhaar KEY ID as a String
- OCTUS_AADHAR_OFFLINE_KEY_SEC -  Accept aadhaar KEY SECRET as a String


#### In-App SDK configuration. 

This part of the integration allows you to integrate the drop-in screens for the in-app experience whereby the Aadhaar holder will enter the Aadhaar Number or VID, Captcha, OTP and four digit share code all within your App without redirecting to the UIDAI website. The Aadhaar Offline file once downloaded will be parsed in-memory and displayed in the App (data shared with the App as JSON data). The experience is seamless with the in-app option. However, one of the caveats to this configuration is that any change to the UIDIA website will need to be updated and is a reactive release and there is a risk of your App not working until the changes reflect in the SDK.


#### Redirect to File SDK configuration. 

This part of the integration will allow the user to upload the file if already downloaded and stored in the user’s device or redirect the user to the UIDIAI website and allow the user to follow the instructions to download the file. The user can enter the Aadhaar Number or VID, captcha, OTP and four digit share code in the website. Once the data is validated by UIDAI, a ZIP file (password protected using the share code) will be downloaded into the resident’s device. The user will now have to switch back to your App and select the file from the devices download folders which will parse the data and display them in the App (data shared with the App as JSON data). As this is the preferred way by UIDAI, the risk of failing even with the changes is negated to a large extent.


## Help
For any queries/feedback, contact us at `support@frslabs.com` 
