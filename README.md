OHHTTPStubs
===========

[![Platform](http://cocoapod-badges.herokuapp.com/p/OHHTTPStubs/badge.png)](http://cocoadocs.org/docsets/OHHTTPStubs)
[![Version](http://cocoapod-badges.herokuapp.com/v/OHHTTPStubs/badge.png)](http://cocoadocs.org/docsets/OHHTTPStubs)
[![Carthage compatible](https://img.shields.io/badge/Carthage-compatible-4BC51D.svg?style=flat)](https://github.com/Carthage/Carthage)
[![Build Status](https://travis-ci.org/AliSoftware/OHHTTPStubs.svg?branch=master)](https://travis-ci.org/AliSoftware/OHHTTPStubs)

`OHHTTPStubs` is a library designed to stub your network requests very easily. It can help you:

* test your apps with **fake network data** (stubbed from file) and **simulate slow networks**, to check your application behavior in bad network conditions
* write **Unit Tests** that use fake network data from your fixtures.

It works with `NSURLConnection`, new iOS7/OSX.9's `NSURLSession`, `AFNetworking` (both 1.x and 2.x), or any networking framework that use Cocoa's URL Loading System.

[![Donate](http://www.paypalobjects.com/en_US/i/btn/btn_donate_LG.gif)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=TRTU3UEWEHV92 "Donate")

----

# Documentation & Usage Examples

`OHHTTPStubs` headers are fully documented using Appledoc-like / Headerdoc-like comments in the header files. You can also [read the **online documentation** here](http://cocoadocs.org/docsets/OHHTTPStubs)
[![Version](http://cocoapod-badges.herokuapp.com/v/OHHTTPStubs/badge.png)](http://cocoadocs.org/docsets/OHHTTPStubs)

## Basic example

<details>
<summary>In Objective-C</summary>

```objc
[OHHTTPStubs stubRequestsPassingTest:^BOOL(NSURLRequest *request) {
  return [request.URL.host isEqualToString:@"mywebservice.com"];
} withStubResponse:^OHHTTPStubsResponse*(NSURLRequest *request) {
  // Stub it with our "wsresponse.json" stub file (which is in same bundle as self)
  NSString* fixture = OHPathForFile(@"wsresponse.json", self.class);
  return [OHHTTPStubsResponse responseWithFileAtPath:fixture
            statusCode:200 headers:@{@"Content-Type":@"application/json"}];
}];
```

</details>

<details open=1>
<summary>In Swift</summary>

This example is using the Swift helpers found in `OHHTTPStubsSwift.swift` provided by the `OHHTTPStubs/Swift` subspec.
 
```swift
stub(isHost("mywebservice.com")) { _ in
  // Stub it with our "wsresponse.json" stub file (which is in same bundle as self)
  let stubPath = OHPathForFile("wsresponse.json", self.dynamicType)
  return fixture(stubPath!, headers: ["Content-Type":"application/json"])
}
```
</details>

##### Notes 

* Using `OHHTTPStubsSwift.swift` and the `Swift` subspec, you could also compose the matcher functions like this: `stub(isScheme("http") && isHost("myhost")) { … }`
* The response files used above can be recorded with tools like [SWHttpTrafficRecorder](https://github.com/capitalone/SWHttpTrafficRecorder). It can record all three formats that are supported by `OHHTTPStubs`, that is, HTTPMessage, response boby/content file, and Mocktail.  

## More examples & Help Topics
    
* For a lot more examples, see the dedicated "[Usage Examples](https://github.com/AliSoftware/OHHTTPStubs/wiki/Usage-Examples)" wiki page.
* The wiki also contain [some articles that can help you get started](https://github.com/AliSoftware/OHHTTPStubs/wiki) with (and troubleshoot if needed) `OHHTTPStubs`.

# Compatibility

`OHHTTPStubs` is compatible with **iOS 5.0+** and **OSX 10.7+**.

`OHHTTPStubs` also works with iOS7's and OSX 10.9's `NSURLSession` mechanism.

`OHHTTPStubs` is **fully compatible with Swift 2.2, 2.3 and 3.0**.

_[Nullability annotations](https://developer.apple.com/swift/blog/?id=25) have also been added to the ObjC API to allow a cleaner API when used from Swift even if you don't use the dediated Swift API wrapper provided by `OHHTTPStubsSwift.swift`._


# Installing in your projects

## CocoaPods

Using [CocoaPods](https://guides.cocoapods.org) is the recommended way.

Simply add `pod 'OHHTTPStubs'` to your `Podfile`. That default subspec includes the `Core` subspec + the `NSURLSession`, `JSON` and `OHPathHelper` subspecs.

If you **intend to use the Swift API** of `OHHTTPStubs`, you should **add** `OHHTTPStubs/Swift` to your `Podfile` as well, so that you'll get the wrapper that brings you a more Swifty API:

```ruby
pod 'OHHTTPStubs' # Default subspecs, including support for NSURLSession & JSON etc
pod 'OHHTTPStubs/Swift' # Adds the Swiftier API wrapper too
```

### Available subspecs

`OHHTTPStubs` is split into subspecs so that when using Cocoapods, you can get only what you need, no more, no less.

* The default subspec includes `NSURLSession`, `JSON`, and `OHPathHelpers`
* The `Swift` subspec adds the Swiftier API (but doesn't include `NSURLSession` & `JSON` by itself)
* `HTTPMessage` and `Mocktail` are opt-in subspecs: list them explicitly if you need them
* `OHPathHelpers` doesn't depend on `Core` and can be used independently of `OHHTTPStubs` altogether

<details>
<summary>List of all the subspecs & their dependencies</summary>

| Subspec | Core  | NSURLSession | JSON  | Swift | OHPathHelpers | HTTPMessage | Mocktail |
| ------- | :---: | :----------: | :---: | :---: | :-----------: | :---------: | :------: |
| `pod 'OHHTTPStubs'` | ✅ | ✅ | ✅ |   | ✅ |   |   |
| `pod 'OHHTTPStubs/Default'` | ✅ | ✅ | ✅ |   | ✅ |   |   |
| `pod 'OHHTTPStubs/Swift'` | ✅ |   |   | ✅ |   |   |   |
| `pod 'OHHTTPStubs/Core'` | ✅ |   |   |   |   |   |   |
| `pod 'OHHTTPStubs/NSURLSession'` | ✅ | ✅ |   |   |   |   |   |
| `pod 'OHHTTPStubs/JSON'` | ✅ |   | ✅ |   |   |   |   |
| `pod 'OHHTTPStubs/OHPathHelpers'` |   |   |   |   | ✅ |   |   |
| `pod 'OHHTTPStubs/HTTPMessage'` | ✅ |   |   |   |   | ✅ |   |
| `pod 'OHHTTPStubs/Mocktail'` | ✅ |   |   |   |   |   | ✅ |

</details>

## Carthage

`OHHTTPStubs` is also be compatible with Carthage. Just add it to your `Cartfile`.

_Note: The `OHHTTPStubs.framework` built with Carthage will include **all** features of `OHHTTPStubs` turned on (in other words, all subspecs of the pod), including `NSURLSession` and `JSON` support, `OHPathHelpers`, `HTTPMessage` and `Mocktail` support, and the Swiftier API._

> Be warned that I don't personally use Carthage, so I won't be able to guarantee much help/support for it.

## Using the right Swift Version of `OHHTTPStubs` for your project

`OHHTTPStubs` support both Swift 2.2 (Xcode 7), Swift 2.3 and Swift 3.0 (Xcode 8) 🎉 

Here's some details about the correct setup you need depending on how you integrated `OHHTTPStubs` into your project.

<details>
<summary><b>CocoaPods</b></summary>

If you use CocoaPods version [`1.1.0.beta.1`](https://github.com/CocoaPods/CocoaPods/releases/tag/1.1.0.beta.1) or later, then CocoaPods will compile `OHHTTPStubs` with the right Swift Version matching the one you use for your project automatically.

For more info, see [CocoaPods/CocoaPods#5540](https://github.com/CocoaPods/CocoaPods/pull/5540) and [CocoaPods/CocoaPods#5760](https://github.com/CocoaPods/CocoaPods/pull/5760).
</details>

<details>
<summary><b>Carthage</b></summary>

The project is currently setup with `SWIFT_VERSION=2.3` on `master`.

This means that the framework on `master` will build using Swift 2.2 on Xcode 7 and Swift 2.3 on Xcode 8.

If you want Carthage to build the framework with Swift 3.0, you can use the `swift-3.0` branch, whose only difference with `master` is that the project's Build Settings set `SWIFT_VERSION=3.0` instead of `2.3`.

_Note:_ Later, probably a few weeks after the Xcode 8 official release, we plan to merge the `swift-3.0` branch into `master` and create a _compatibility branch_ `swift-2.3` for Carthage users who have not migrated their Swift code yet. We'll try to keep the branch up-to-date with master as best we can.

Hopefully, Carthage will address this in a future release. See the related issue [Carthage/Carthage#1445](https://github.com/Carthage/Carthage/issues/1445).
</details>

# Special Considerations

## Using OHHTTPStubs in your Unit Tests

`OHHTTPStubs` is ideal to write Unit Tests that normally would perform network requests. But if you use it in your Unit Tests, don't forget to:

* remove any stubs you installed after each test — to avoid those stubs to still be installed when executing the next Test Case — by calling `[OHHTTPStubs removeAllStubs]` in your `tearDown` method. [see this wiki page for more info](https://github.com/AliSoftware/OHHTTPStubs/wiki/Remove-stubs-after-each-test)
* be sure to wait until the request has received its response before doing your assertions and letting the test case finish (like for any asynchronous test). [see this wiki page for more info](https://github.com/AliSoftware/OHHTTPStubs/wiki/OHHTTPStubs-and-asynchronous-tests)

## Automatic loading

Thanks to method swizzling, `OHHTTPStubs` is automatically loaded and installed both for:

* requests made using `NSURLConnection` or `[NSURLSession sharedSession]`;
* requests made using a `NSURLSession` created using a `[NSURLSessionConfiguration defaultSessionConfiguration]` or `[NSURLSessionConfiguration ephemeralSessionConfiguration]` configuration (using `[NSURLSession sessionWithConfiguration:…]`-like methods).

If you need to disable (and re-enable) `OHHTTPStubs` — globally or per `NSURLSession` — you can use `[OHHTTPStubs setEnabled:]` / `[OHHTTPStubs setEnabled:forSessionConfiguration:]`.

## Known limitations

* `OHHTTPStubs` **can't work on background sessions** (sessions created using `[NSURLSessionConfiguration backgroundSessionConfiguration]`) because background sessions don't allow the use of custom `NSURLProtocols` and are handled by the iOS Operating System itself.
* `OHHTTPStubs` don't simulate data upload. The `NSURLProtocolClient` `@protocol` does not provide a way to signal the delegate that data has been **sent** (only that some has been loaded), so any data in the `HTTPBody` or `HTTPBodyStream` of an `NSURLRequest`, or data provided to `-[NSURLSession uploadTaskWithRequest:fromData:];` will be ignored, and more importantly, the `-URLSession:task:didSendBodyData:totalBytesSent:totalBytesExpectedToSend:` delegate method will never be called when you stub the request using `OHHTTPStubs`.

_As far as I know, there's nothing we can do about those two limitations. Please let me know if you know a solution that would make that possible anyway._


## Submitting to the AppStore

`OHHTTPStubs` **can be used** on apps submitted **on the AppStore**. It does not use any private API and nothing prevents you from shipping it.

But you generally only use stubs during the development phase and want to remove your stubs when submitting to the AppStore. So be careful to only include `OHHTTPStubs` when needed (only in your test targets, or only inside `#if DEBUG` sections, or by using [per-Build-Configuration pods](https://guides.cocoapods.org/syntax/podfile.html#pod)) to avoid forgetting to remove it when the time comes that you release for the AppStore and you want your requests to hit the real network!



# License and Credits

This project and library has been created by Olivier Halligon (@aligatr on Twitter) and is under the MIT License.

It has been inspired by [this article from InfiniteLoop.dk](http://www.infinite-loop.dk/blog/2011/09/using-nsurlprotocol-for-injecting-test-data/).

I would also like to thank Kevin Harwood ([@kcharwood](https://github.com/kcharwood)) for migrating the code to `NSInputStream`, Jinlian Wang ([@JinlianWang](https://github.com/JinlianWang)) for adding Mocktail support, and everyone else who contributed to this project on GitHub somehow.

If you want to support the development of this library, feel free to [![Donate](http://www.paypalobjects.com/en_US/i/btn/btn_donate_LG.gif)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=TRTU3UEWEHV92 "Donate"). Thanks to all contributors so far!
