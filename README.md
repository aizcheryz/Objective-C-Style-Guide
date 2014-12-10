# Objective-C-Style-Guide
=======================
Objective-C Style Guide for iOS Development.

## Table of Contents
* [Project Structure](#project-structure)
* [Code Structure](#code-structure)
* [Header Prefix](#header-prefix)
* [Comments](#comments)
* [Property Attribute](#property-attribute)
* [Private Property](#private-property)
* [Naming](#naming)
* [Naming Underscore](#naming-underscore)
* [Image Naming](#image-naming)
* [Constant](#constant)
* [Enumerated Type](#enumerated-type)
* [Bitmask](#bitmask)
* [Variable](#variable)
* [Variable Literals](#variable-Literals)
* [Init Methods](#init-methods)
* [Constructor Methods](#constructor-methods)
* [CGRect Methods](#cgrect-methods)
* [Dot-Notation Syntax](#dot-notation-syntax)
* [Operators](#operators)
* [Control Structure - Conditional](#conditional)
* [Control Structure - Iteration](#iteration)
* [Error Handling](#error-handling)
* [Singleton](#singleton)
* [Line Break](#line-break)
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