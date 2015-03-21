# EMPageViewController

## A better page view controller for iOS
EMPageViewController is a full replacement for UIPageViewController with the features and predictability you’ve always wanted from a page view controller.

### Delegate messages every step of the way
EMPageViewController gives your delegate messages for every step of the page transition process: before, during, and after. This makes it very easy for you to incorporate animations or any other events that are highly dependent on the progress of transitioning a page.

### Convenient navigation methods
In addition to the ability scroll to any view controller, you can also easily scroll to the next or previous page without gestures if your app has navigation buttons.

### Written in Swift
EMPageViewController is not a subclass of UIPageViewController. Instead, it's a subclass of UIViewController with a UIScrollView, written in Swift, and it has common sense delegate and data source methods that will make the development of your page-based iOS app a breeze.

## Compatibility
* Xcode 6.1+
* iOS 7+
* iPhone, iPad, and iPod Touch

## License
[MIT License](LICENSE)

## Installation
Simply include [EMPageViewController.swift](EMPageViewController.swift) into your project.

## Demo
To see EMPageViewController in action, clone this repository, open the Xcode project file *Greetings.xcodeproj* in [Examples/Greetings](Examples/Greetings), and build and run the project.

## Usage

### Initialization

Initialize EMPageViewController and set its `dataSource` and `delegate` properties.

```swift
let pageViewController = EMPageViewController()
pageViewController.dataSource = self
pageViewController.delegate = self
```

You'll also need to adopt the `EMPageViewControllerDataSource` and `EMPageViewControllerDelegate` protocols. Instructions on how to conform to these protocols later.
```swift
class ViewControllerSubclass: UIViewController, EMPageViewControllerDataSource, EMPageViewControllerDelegate ...
...
```

Set the initial view controller for the page view controller with `selectViewController:direction:animated:completion:`. This is the page that will be selected when your view controller loads.
```swift
let initialViewController = MyViewController() // You'll probably have a method here that returns your view controller based on an index value or something similar, like viewControllerAtIndex:
pageViewController.selectViewController(currentViewController, direction: .Forward, animated: false, completion: nil)
```

Add EMPageViewController as a child view controller and subview to the UIViewController and UIView it will reside in.
```swift
self.addChildViewController(pageViewController)
self.view.addSubview(pageViewController)
pageViewController.didMoveToParentViewController(self)
```

Lastly, don't forget to set your newly created EMPageViewController object to a property in your UIViewController subclass to it's retained for use later on.

```swift
self.pageViewController = pageViewController
```


## Documentation

### EMPageViewController

#### Properties

* * *

##### `dataSource`

The object that provides view controllers on an as-needed basis throughout the navigation of the page view controller.

**Declaration**
```swift
weak var dataSource:EMPageViewControllerDataSource!
```

* * *

##### `delegate`

The object that recieves messages throughout the navigation of the page view controller.

**Declaration**
```swift
weak var delegate:EMPageViewControllerDelegate?
```

* * *

#### Methods

* * *

##### `selectViewController:direction:animated:completion:`

**Declaration**
```swift
func selectViewController(viewController: UIViewController, direction: EMPageViewControllerNavigationDirection, animated: Bool, completion: ((transitionSuccessful: Bool) -> Void)?)
```

* * *

##### `scrollForward:completion:`

**Declaration**
```swift
func scrollForward(animated: Bool, completion: ((transitionSuccessful: Bool) -> Void)?)
```

* * *

##### `scrollReverse:completion:`

**Declaration**
```swift
func scrollReverse(animated: Bool, completion: ((transitionSuccessful: Bool) -> Void)?)
```

* * * 

### EMPageViewControllerDataSource
The `EMPageViewControllerDataSource` protocol is adopted to provide the view controllers that are displayed when the user scrolls through pages. Methods are called on an as-needed basis.

Each method returns a UIViewController object or nil if there are no view controllers to be displayed.

If the data source is nil, gesture based scrolling will be disabled and all view controllers must be provided through `selectViewController:direction:animated:completion:`.

* * *

##### `em_pageViewController:viewControllerLeftOfViewController:`

Called to optionally return a view controller that is to the left of a given view controller.

**Declaration**
```swift
func em_pageViewController(pageViewController: EMPageViewController, viewControllerLeftOfViewController viewController: UIViewController) -> UIViewController?
```
**Parameters**

Parameter              | Description
---------------------- | --------------------------------------
*`pageViewController`* | The page view controller
*`viewController`*     | The point of reference view controller

**Return value**

The view controller that is to the left of the given `viewController`, or `nil` if there is no view controller to be displayed.

* * *

##### `em_pageViewController:viewControllerRightOfViewController:`

Called to optionally return a view controller that is to the right of a given view controller.

**Declaration**
```swift
func em_pageViewController(pageViewController: EMPageViewController, viewControllerRightOfViewController viewController: UIViewController) -> UIViewController?
```
**Parameters**

Parameter              | Description
---------------------- | --------------------------------------
*`pageViewController`* | The page view controller
*`viewController`*     | The point of reference view controller

**Return value**

The view controller that is to the right of the given `viewController`, or `nil` if there is no view controller to be displayed.

* * *

### EMPageViewControllerDelegate

* * *

##### `em_pageViewController:willStartScrollingFrom:destinationViewController:`

**Declaration**
```swift
optional func em_pageViewController(pageViewController: EMPageViewController, willStartScrollingFrom startingViewController: UIViewController, destinationViewController: UIViewController)
```
This method will not be called if the starting view controller is `nil`.

* * *

##### `em_pageViewController:isScrollingFrom:destinationViewController:progress:`

**Declaration**
```swift
optional func em_pageViewController(pageViewController: EMPageViewController, isScrollingFrom startingViewController: UIViewController, destinationViewController: UIViewController, progress: CGFloat)
```
This method will not be called if the starting view controller is `nil`.

* * *

##### `em_pageViewController:didFinishScrollingFrom:selectedViewController:transitionSuccessful:`

**Declaration**
```swift
optional func em_pageViewController(pageViewController: EMPageViewController, didFinishScrollingFrom previousViewController: UIViewController?, selectedViewController: UIViewController, transitionSuccessful: Bool)
```

* * *

