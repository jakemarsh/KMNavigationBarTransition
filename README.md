KMNavigationBarTransition [中文介绍](https://github.com/MoZhouqi/KMNavigationBarTransition/blob/master/README_CN.md)
============

A drop-in universal library helps you to manage the navigation bar styles and makes transition animations smooth between different navigation bar styles while pushing or popping a view controller for all orientations. And you don't need to write any line of code for it, it all happens automatically.

## Screenshots

### Before

![KMNavigationBarTransition](https://raw.githubusercontent.com/MoZhouqi/KMNavigationBarTransition/master/Screenshots/Before1.gif)
![KMNavigationBarTransition](https://raw.githubusercontent.com/MoZhouqi/KMNavigationBarTransition/master/Screenshots/Before2.gif)

### Now

![KMNavigationBarTransition](https://raw.githubusercontent.com/MoZhouqi/KMNavigationBarTransition/master/Screenshots/Now1.gif)
![KMNavigationBarTransition](https://raw.githubusercontent.com/MoZhouqi/KMNavigationBarTransition/master/Screenshots/Now2.gif)

## Introduction

The design concept of the library is that what you only need to care about is the background style of the navigation bar in the *current* view controller, without handling the various background styles while pushing or popping.

The library can capture the background style of the navigation bar in the disappearing view controller when pushing, and when you pop back to the view controller, the navigation bar will restore the previous style, so you don't need to consider the background style after popping. And you also don't need to consider it after pushing, because it is the view controller to be pushed that needs to be considered.

## Usage

You don't need to import any header file when using this library, the library uses [Method Swizzling](http://nshipster.com/method-swizzling/) to achieve the effect.

It is recommended to set the default background style of the navgaion bar in the `viewDidLoad` method of the base view controller. When you need to change it, generally, you only need to do it in the `viewDidLoad` method of the *current* view controller.

The following are some suggestions to set the background style of the navigation bar, and you can see the Example for details.

- There are two methods to set the background style of the navigation bar, `setBackgroundImage:forBarMetrics:` and `setBarTintColor:`. It is recommended to use the former, you can see [Known Issues](#known-issues) for the details.

- It is better not to change the value of the `translucent` property arbitrarily after initialization, otherwise the interface layout would be prone to confusion.

- When the value of the `translucent` property is `true`, you can use the following mehod to make the navigation bar transparent:

  ```swift
  navigationController?.navigationBar.setBackgroundImage(UIImage(), forBarMetrics: .Default)
  navigationController?.navigationBar.shadowImage = UIImage() // shadowImage is the 1px line
  ```

- You can change the alpha value of the background color of the navigaiton bar by changing the alpha value of the image in the `setBackgroundImage:forBarMetrics:` method.

- You can use the following method to show or hide the navigation bar in `viewWillAppear:`:

  ```swift
  override func viewWillAppear(animated: Bool) {
      super.viewWillAppear(animated)
      navigationController?.setNavigationBarHidden(hidden, animated: animated)
  }
  ```

  You'd better not do it in neither `viewWillDisappear:` nor other methods performing transitions, because it is not easy to manage. Again, what you only need to care about is the style of the navigation bar in the *current* view controller.

  Of course, you'd better not hide the navigaion bar, it might triggers some apple's bug with interactive pop gesture.

## Installation

### CocoaPods

You can install the latest release version of CocoaPods with the following command:

```bash
$ gem install cocoapods
```

Simply add the following line to your Podfile:

```ruby
source 'https://github.com/CocoaPods/Specs.git'
platform :ios, '7.0' 

pod 'KMNavigationBarTransition', '~> 0.0.5' 
```

Then, run the following command:

```bash
$ pod install
```

## Requirements

- iOS 7.0+

## Known Issues

On iOS 8.2 or below, if you set the value of the `translucent` property to `true` and set the `barTintColor` for the background color, and then change the `barTintColor`, the background color of the navigation bar will flash when the interactive transition is cancelled.

To avoid this from happening, it is recommended to use `setBackgroundImage:forBarMetrics:` instead of `setBarTintColor:` to change the background color of the navigation bar. 

## License

KMNavigationBarTransition is released under the MIT license. See LICENSE for details.