# Objective-C-Style-Guide
Objective-C Style Guide for iOS Development.

## Table of Contents
* [Project Structure](#project-structure)
* [Code Structure](#code-structure)
* [Header Prefix](#header-prefix)
* [Comments](#comments)
* [Property Attribute](#property-attribute)
* [Naming](#naming)
* [Naming Underscore](#naming-underscore)
* [Constant](#constant)
* [Enumerated Type](#enumerated-type)
* [Variable](#variable)
* [Variable Literals](#variable-Literals)
* [Init Methods](#init-methods)
* [Constructor Methods](#constructor-methods)
* [Dot-Notation Syntax](#dot-notation-syntax)
* [Operators](#operators)
* [Conditional](#conditional)
	* [If](#if)
	* [Switch](#switch)
* [Iteration](#iteration)
	* [For](#for)
	* [While](#while)
* [Error Handling](#error-handling)
* [Singleton](#singleton)
* [3rd Party Libraries](#3rd-party-libraries)
* [Reference](#reference)

## Project Structure
- On Progress

## Code Structure
[Pragma marks](http://nshipster.com/pragma/) are a great way to group your methods, especially in view controllers. Here is a common structure that works with almost any view controller:

```objective-c

#import <SomeExternalLibrary/SomeExternalLibraryHeader.h>
#import "SomeHelper.h"
#import "SomeJSONModel.h"
#import "SomeModel.h"
#import "SomeController.h"
#import "SomeViewController.h"


static NSString * const XYZFooStringConstant = @"FoobarConstant";
static CGFloat const XYZFooFloatConstant = 1234.5;


@interface XYZFooViewController () <XYZBarDelegate>

@required
@property (nonatomic, copy)
- (void)fooBar;

@optional
@property (nonatomic, copy)
- (void)fooBar;

@end


@implementation XYZFooViewController

#pragma mark - Lifecycle
- (instancetype)initWithFoo:(Foo *)foo;
- (void)dealloc;

#pragma mark - View Lifecycle
- (void)viewDidLoad;
- (void)viewWillAppear:(BOOL)animated;
...
- (void)didReceiveMemoryWarning {}

#pragma mark - Custom Accessors
- (void)setCustomProperty:(id)value {}
- (id)customProperty {}
...

#pragma mark - Public Interface
- (void)initView;
- (void)initData;
- (void)startAnimating;
- (void)stopAnimating;
...

#pragma mark - Auto Layout
- (void)makeViewConstraints;
...

#pragma mark - IBActions
- (IBAction)submitData:(id)sender {}
- (IBAction)storeNameTextFieldOnChange:(id)sender;
- (IBAction)storeSubmitButtonOnTouch:(id)sender;
- (void)foobarButtonOnTouch;
- (void)foobarSwitchOnChange;

#pragma mark - Helpers
- (NSString *)displayNameForFoo:(Foo *)foo;

#pragma mark - Public Methods
- (void)publicMethod {}
...

#pragma mark - Private Methods
- (void)privateMethod {}
...

#pragma mark - Delegates
#pragma mark - Protocol conformance
#pragma mark - UITextFieldDelegate
#pragma mark - UISearchBarDelegate
#pragma mark - UITableViewDataSource
#pragma mark - UITableViewDelegate
...

```

### Header Prefix

Adding frameworks that are used in the majority of a project to a header prefix is preferred. If these frameworks are in the header prefix, they should **never** be imported in source files in the project.

For example, if a header prefix looks like the following:

```objective-c
#ifdef __OBJC__
    #import <Foundation/Foundation.h>
    #import <UIKit/UIKit.h>
    #import <QuartzCore/QuartzCore.h>
    ...
    #import <GlobalAPIController/GlobalAPIController.h>
#endif
```

## Comments

When they are needed, comments should be used to explain a function or **why** a particular piece of code does something.
```objective-c

/*
 * Function for what
 * Parameters	: @[Parameters 1, Parameters 2, Parameters 3, ...]
 * Return		: ...
 */

- (NSMutableArray *)getLastUpdateData:(NSDate *)updatedAt {

	// comparing data from server
	...
}


```

## Property Attributes

Property attributes should be explicitly listed, and will help new programmers when reading the code.  The order of properties should be storage then atomicity, which is consistent with automatically generated code when connecting UI elements from Interface Builder.

**Preferred:**

```objc
@interface StoreController ()

@property (strong, nonatomic) NSString *storeName;
@property (weak, nonatomic) IBOutlet UIView *emptyView;
@property (weak, nonatomic) IBOutlet UIView *listView;
@end
```

**Not Preferred:**

```objc
@property (nonatomic, weak) IBOutlet UIView *emptyView;
@property (nonatomic) NSString *storeName;

@property (nonatomic) NSString *storeName;
@property (nonatomic, weak) IBOutlet UIView *emptyView;
```

## Naming

Keep naming variable or function name consistent.

**Preferred:**

```objc

UIButton *settingsButton;
- (void)loadView;
- (UINavigationItem *)navigationItem;
+ (UILabel *)labelWithText:(NSString *)text;
_storeName;

```

**Not Preferred:**

```objc

UIButton *settBut;
- (void)LoadView;
- (UINavigationItem *)navItem;
+ (UILabel *)labelText:(NSString *)text;
_Storename;

```

## Underscores

When using properties, instance variables should always be accessed and mutated using `self.`. This means that all properties will be visually distinct, as they will all be prefaced with `self.`. 

An exception to this: inside initializers, the backing instance variable (i.e. _variableName) should be used directly to avoid any potential side effects of the getters/setters.

```objc

_storeName;
_storeURL;

```

**Local variables should not contain underscores.**

## Constants

Keep app-wide constants in a `Constants.h` file that is included in the prefix header.

Instead of preprocessor macro definitions (via `#define`), use actual constants:

    static CGFloat const BrandingFontSizeSmall = 12.0f;
    static NSString * const AwesomenessDeliveredNotificationName = @"foo";

Actual constants have more explicit scope (they’re not available in all imported/included files until undefined), cannot be redefined or undefined in later parts of the code, and are available in the debugger.

## Enumerated Types

When using `enum`s, it is recommended to use the new fixed underlying type specification because it has stronger type checking and code completion. The SDK now includes a macro to facilitate and encourage use of fixed underlying types: `NS_ENUM()`

**Preffered:**

```objc
typedef NS_ENUM(NSInteger, RWTLeftMenuTopItemType) {
  RWTLeftMenuTopItemMain,
  RWTLeftMenuTopItemShows,
  RWTLeftMenuTopItemSchedule
};

typedef NS_ENUM(NSInteger, RWTGlobalConstants) {
  RWTPinSizeMin = 1,
  RWTPinSizeMax = 5,
  RWTPinCountMin = 100,
  RWTPinCountMax = 500,
};
```

Older k-style constant definitions should be **avoided** unless writing CoreFoundation C code (unlikely).

**Not Preferred:**

```objc
enum GlobalConstants {
  kMaxPinSize = 5,
  kMaxPinCount = 500,
};
```

## Variables

**Preferred:**

```objc

@interface NYTSection: NSObject

@property (nonatomic) NSString *headline;

@end

```

**Not Preferred:**

```objc
@interface NYTSection : NSObject {
    NSString *headline;
}
```

## Variable Literals

`NSString`, `NSDictionary`, `NSArray`, and `NSNumber` literals should be used whenever creating immutable instances of those objects. Pay special care that `nil` values not be passed into `NSArray` and `NSDictionary` literals, as this will cause a crash.

**Preferred:**

```objc
NSArray *names = @[@"Aiz", @"Choi", @"Firman", @"Heru", @"Putra", @"Riza"];
NSDictionary *productManagers = @{@"iPhone" : @"Andika", @"iPad" : @"Putra", @"Mobile Web" : @"Vino"};
NSNumber *shouldUseLiterals = @YES;
NSNumber *buildingZIPCode = @65139;
```

**Not Preferred:**

```objc
NSArray *names = [NSArray arrayWithObjects:@"Aiz", @"Choi", @"Firman", @"Heru", @"Putra", @"Riza", nil];
NSDictionary *productManagers = [NSDictionary dictionaryWithObjectsAndKeys: @"Andika", @"iPhone", @"Putra", @"iPad", @"Vino", @"Mobile Web", nil];
NSNumber *shouldUseLiterals = [NSNumber numberWithBool:YES];
NSNumber *buildingZIPCode = [NSNumber numberWithInteger:65139];
```

## Init and Dealloc

`dealloc` methods should be placed at the top of the implementation, directly after the `@synthesize` and `@dynamic` statements. `init` should be placed directly below the `dealloc` methods of any class.

`init` methods should be structured like this:

```objc
- (instancetype)init {
    self = [super init]; // or call the designated initializer
    if (self) {
        // Custom initialization
    }
    return self;
}
```
## Class Constructor Methods

Where class constructor methods are used, these should always return type of 'instancetype' and never 'id'. This ensures the compiler correctly infers the result type. 

```objc
@interface Airplane

+ (instancetype)storeWithUrl:(NSString *)storeURL;

@end
```

## Dot-Notation Syntax

**Preferred:**
```objc
NSInteger arrayCount = [self.array count];
view.backgroundColor = [UIColor orangeColor];
[UIApplication sharedApplication].delegate;
```

**Not Preferred:**
```objc
NSInteger arrayCount = self.array.count;
[view setBackgroundColor:[UIColor orangeColor]];
UIApplication.sharedApplication.delegate;
```

## Operators

**Preferred:**
```objc
NSInteger answer = 42;
answer += 9;
answer++;
answer = 40 + 2;
answer = (value != 0) ? x : y;
```

**Not Preferred:**
```objc
answer+=9;
answer ++;
answer = 40+2;
result = a > b ? x = c > d ? c : d : y;
```


## Conditionals

### if
**Preferred:**
```objc
if (!error) {
    return success;
}
```

**Not Preferred:**
```objc
if (!error)
    return success;
```

or

```objc
if (!error) return success;
```

### switch
**Preferred:**
Braces are not required for case statements, unless enforced by the complier.  
When a case contains more than one line, braces should be added.

```objc
switch (condition) {
  case 1:
    // ...
    break;
  case 2: {
    // ...
    // Multi-line example using braces
    break;
  }
  case 3:
    // ...
    break;
  default: 
    // ...
    break;
}

```

or

```objc
switch (condition) {
  case 1:
    // ** fall-through! **
  case 2:
    // code executed for values 1 and 2
    break;
  default: 
    // ...
    break;
}

```

or

```objc
RWTLeftMenuTopItemType menuType = RWTLeftMenuTopItemMain;

switch (menuType) {
  case RWTLeftMenuTopItemMain:
    // ...
    break;
  case RWTLeftMenuTopItemShows:
    // ...
    break;
  case RWTLeftMenuTopItemSchedule:
    // ...
    break;
}
```

## Iteration

### For

```objc
for (NSInteger i = 0; i < 10; i++) {
    // Do something
}
```

or

```objc

for (NSString *key in dictionary) {
    // Do something
}
```

### While

```objc
while (something < somethingElse) {
    // Do something
}
```

## Error handling

**Preferred:**
```objc
NSError *error;
if (![self trySomethingWithError:&error]) {
  // Handle Error
}
```

**Not Preferred:**
```objc
NSError *error;
[self trySomethingWithError:&error];
if (error) {
  // Handle Error
}
```

## Singletons

```objc
+ (instancetype)sharedInstance {
   static id sharedInstance = nil;

   static dispatch_once_t onceToken;
   dispatch_once(&onceToken, ^{
      sharedInstance = [[self alloc] init];
   });

   return sharedInstance;
}
```

## 3rd Party Libraries
**Networking**
- [AFNetworking](https://github.com/AFNetworking/AFNetworking)

**JSON Serialize**
- [JSONModel](https://github.com/icanzilb/JSONModel)

**Core Data**
- [MagicalRecord](https://github.com/magicalpanda/MagicalRecord)

**UI**
- [TPKeyboardAvoiding](https://github.com/michaeltyson/TPKeyboardAvoiding)
- [MBProgressHUD](https://github.com/jdg/MBProgressHUD)
- [SDWebImage](https://github.com/rs/SDWebImage) [SDWebImage-ProgressView](https://github.com/kevinrenskers/SDWebImage-ProgressView)
- [SimplzActionSheet](https://github.com/aizcheryz/SimplzActionSheet)
- [TTTAttributedLabel](https://github.com/mattt/TTTAttributedLabel)

## Reference
- [NYTimes](https://github.com/NYTimes/objective-c-style-guide)
- [Ray Webderlich](github.com/raywenderlich/objective-c-style-guide)
- [Future Ice](https://github.com/futurice/ios-good-practices)
- [Soffes](https://gist.github.com/soffes/812796);

# Thank you