# ApplicationService


Application service allow you to do put all your code in AppDelegate

You can create your own and be injected by a template

Full example: https://github.com/4d-for-ios/form-login-SignInWithApple

## implement the service

In `Sources/Application/Services/` of one template create a swift file with a class. 

Example MyAppService.swift

```swift
import UIKit
import QMobileUI

// The class instance singleton
@objc(MyAppService)
class MyAppService: NSObject {
    static var instance: MyAppService = MyAppService()
    override init() {}
}

// MARK: Implement the service ApplicationService
extension MyAppService: ApplicationService {

    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) {}

    func applicationDidEnterBackground(_ application: UIApplication) {}

    func applicationWillEnterForeground(_ application: UIApplication) {}

    func applicationDidBecomeActive(_ application: UIApplication) {}

    func applicationWillResignActive(_ application: UIApplication) {}

    func applicationWillTerminate(_ application: UIApplication) {}

    func applicationDidReceiveMemoryWarning(_ application: UIApplication) {}

    func application(_ application: UIApplication, didRegisterForRemoteNotificationsWithDeviceToken deviceToken: Data) {}

    func application(_ application: UIApplication, open url: URL, options: [UIApplication.OpenURLOptionsKey: Any]) {}

}
```

## manifest.json

in manifest provide the name of service

```json
{
   ...
  "inject": true,
  "capabilities": {
        "settings": {
        	"application.services": [
        		"MyAppService"
        	]
        }
    }  
}
```
