# CocoaPods plugin for quickly adding Sequencing.com's OAuth2 and File Selector to iOS apps coded in Objective-C

=========================================
This repo contains CocoaPods plugin code for implementing Sequencing.com's OAuth2 authentication and File Selector for your Swift iOS app so that your app can securely access [Sequencing.com's](https://sequencing.com/) API and app chains.

* oAuth flow is explained [here](https://github.com/SequencingDOTcom/OAuth2-code-with-demo)
* Example that uses this Pod is located [here](https://github.com/SequencingDOTcom/OAuth2-code-with-demo/tree/master/objective-c)

Contents
=========================================
* Cocoa Pod integration
* Resources
* Maintainers
* Contribute

Cocoa Pod integration
======================================

You need to follow instruction below if you want to install and use OAuth logic and file selector logic in your existed or new project.

* create a new project in Xcode
* install pod (see instruction here https://cocoapods.org > getting started)
	* create Podfile in your project directory:
```$ pod init```
    * specify "sequencing-oauth-api-objc" pod parameters:
```pod 'sequencing-oauth-api-objc', '~> 1.0.1'```
	* install the dependency in your project:
```$ pod install```
	* always open the Xcode workspace instead of the project file:
```$ open *.xcworkspace```
* use authorization method(s)
    * add import ```#import "SQOAuth.h"```
	* for authorization you need to specify your application parameters in NSString format (BEFORE using authorization methods) 
		```
		static NSString *const CLIENT_ID	 = @"your CLIENT_ID here";
		static NSString *const CLIENT_SECRET = @"your CLIENT_SECRET here";
		static NSString *const REDIRECT_URI	 = @"REDIRECT_URI here";
		static NSString *const SCOPE         = @"SCOPE here";
		```    

	* next step you need to register these parameters into application instance
		```
		[[SQOAuth sharedInstance] 
		registrateApplicationParametersCliendID:CLIENT_ID
		ClientSecret:CLIENT_SECRET
		RedirectUri:REDIRECT_URI
		Scope:SCOPE];
		```
	* you can authorize your user then (e.g. via "login" button)
	For authorization you can use either "authorizeUser" or "authorizeUserAndGetToken" method (via shared instance init):

		1.

		```
		[[SQOAuth sharedInstance] authorizeUser:^(SQAuthResult *result) {
			// your code here
		}];
		```
		
		```authorizeUser``` will return a block with "SQAuthResult" shared instance.
		```SQAuthResult``` - is a shared instance that contains 2 properties:
		* ```SQToken *token```		- it is a always up-to-date token
		* ```BOOL isAuthorized```	- property that indicates user s authorized status
			
		Pay attention that you do not need to care about token refresh. It's handled automatically in auth logic.
		There is an internal method that verifies if token is expired and refreshes it.
		
		2.

		```
		[[SQOAuth sharedInstance] authorizeUserAndGetToken:^(SQToken *token) {
			// your code here
		}];
		```
		
		```authorizeUserAndGetToken``` will return a block with object of SQToken class directly.
		SQToken object contains following 5 properties with clear titles for usage:

		```	
		NSString *accessToken
		NSDate   *expirationDate
		NSString *tokenType
		NSString *scope
		NSString *refreshToken
		```

* use file selector method(s)
	* add import 
	```#import "SQAPI.h"```
	* you can load/get files, list of own files or list of sample files, via methods "loadOwnFiles" and "loadSampleFiles" (via shared instance init):

		1.
		```
		[[SQAPI sharedInstance] loadOwnFiles:^(NSArray *myFiles) {
			// your code here
		}];
		```
		"loadOwnFiles" will return a block with NSArray of dictionary objects with file details inside.

		2.
		```
		[[SQAPI sharedInstance] loadSampleFiles:^(NSArray *sampleFiles) {
			// your code here
		}];
		```
    	
	    	"loadSampleFiles" will return a block with NSArray of dictionary objects with file details inside.
    
	* each file contains following keys and values:

		```
		DateAdded:		"string value"
		Ext:			"string value"
		FileCategory:	"string value"
		FileSubType:	"string value"
		FileType:		"string value"
		FriendlyDesc1:	"string value"
		FriendlyDesc2:	"string value"
		Id:				"string value"
		Name:			"string value"
		Population:		"string value"
		Sex:			"string value"
		```

Resources
======================================
* [App chains](https://sequencing.com/app-chains)
* [File selector code](https://github.com/SequencingDOTcom/File-Selector-code)
* [Developer center](https://sequencing.com/developer-center)
* [Developer documentation](https://sequencing.com/developer-documentation/)

Maintainers
======================================
This repo is actively maintained by [Sequencing.com](https://sequencing.com/). Email the Sequencing.com bioinformatics team at gittaca@sequencing.com if you require any more information or just to say hola.

Contribute
======================================
We encourage you to passionately fork us. If interested in updating the master branch, please send us a pull request. If the changes contribute positively, we'll let it ride.
