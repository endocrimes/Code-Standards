# My Objective-C Style guide

This style guide outlines the coding conventions that I try to stick to when writing Objective-C. I'm posting it here mostly as a brain dump and easy reference for the future, and to formalise it a little more.

It's pretty similar to that of the [NYTimes](https://github.com/NYTimes/objective-c-style-guide). This document is mostly a customised version of that, you should go check theirs out!

## Useful Resources

If you're looking to see some of the reasons behind some choices, or for something I haven't covered, look at the sites below, Apples documentation is pretty great.

* [The Objective-C Programming Language](http://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/ObjectiveC/Introduction/introObjectiveC.html)
* [Cocoa Fundamentals Guide](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CocoaFundamentals/Introduction/Introduction.html)
* [Coding Guidelines for Cocoa](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)
* [iOS App Programming Guide](http://developer.apple.com/library/ios/#documentation/iphone/conceptual/iphoneosprogrammingguide/Introduction/Introduction.html)

## Table of Contents

* [Comments](#comments)
* [Dot-Notation Syntax](#dot-notation-syntax)
* [Categories](#categories)
* [Spacing](#spacing)
* [Conditionals](#conditionals)
* [Ternary Operator](#ternary-operator)
* [Error handling](#error-handling)
* [Methods](#methods)
* [Variables](#variables)
* [Naming](#naming)
* [Comments](#comments)
* [Init & Dealloc](#init-and-dealloc)
* [Literals](#literals)
* [Constants](#constants)
* [Enumerated Types](#enumerated-types)
* [Private Properties](#private-properties)
* [Image Naming](#image-naming)
* [Booleans](#booleans)
* [Singletons](#singletons)
* [Xcode Project](#xcode-project)

## Comments

### Single-Line Comments

In single-line comments, the `//` should always be followed by one space.

#### Example

```objc
// this is a comment
```

**Not:**

```objc
//this is a comment
```

### Multi-Line Comments

In multi-line comments, `/*` and `*/` should each be one their own line, with the comment body on one or more lines between them. Each line in between should begin with a `*`, followed by one space. All of the `*`s of a mulit-line comment should line up - each line of a comment after the first line should be indented by one extra space. Blank lines are permissible in multi-line comments, but they should still be prefixed with a `*`. The line after `/*` and the line before `*/` should not be blank.

#### Example

```objc
/*
 * This is a comment.
 *
 * Still a comment.
 */
```

**Not:**

```objc
/* This is a comment.
Still a comment. */
```

### Differences between Single- and Multi-Line Comments

Single line comments should be used if the content of the comment is a single sentence and it fits comfortably on a single line. In all other circumstances, a multi-line comment should be used.

Single-line comments are not required to start with a capital letter, and they should only do so if the first word of the comment would still be capitalised in the middle of the sentence. Single line comments should only end with a punctuation mark in the case of question marks.

Multi-line comments should be composed of proper English sentences with capitalised first letters and all appropriate punctuation marks should be present. Lines of a multi-line comment should be wrapped at *apporximately* 80 characters for readability, depending on the context.

## Dot-Notation Syntax

Dot-notation should **always** be used for accessing and mutating properties. Bracket notation is preferred in all other instances.

**For example:**
```objc
view.backgroundColor = [UIColor orangeColor];
[UIApplication sharedApplication].delegate;
```

**Not:**
```objc
[view setBackgroundColor:[UIColor orangeColor]];
UIApplication.sharedApplication.delegate;
```

## Categories

Method names in categories should only be prefixed if they are implementation specific. In all other cases, the implementation of a method should not be of importance. 

**For example:**
```objc
id object = [array firstObject]; 
```
Should work regardless of its implementation detail.

## Spacing

* Indent using 4 spaces. Never indent with tabs. Be sure to set this preference in Xcode. 
* Method braces and other braces (`if`/`else`/`switch`/`while` etc.) always open on a new line.

**For example:**
```objc
if (user.isHappy) 
{
//Do something
}
else 
{
//Do something else
}
```
* There should be exactly one blank line between methods to aid in visual clarity and organisation. Whitespace within methods should separate functionality, but often there should probably be new methods.
* `@synthesize` and `@dynamic` should each be declared on new lines in the implementation.

## Conditionals

Conditional bodies should always use braces even when a conditional body could be written without braces (e.g., it is one line only) to prevent errors. It's also more easy to quickly scan (as it keeps the style of other conditionals).

**For example:**
```objc
if (!error) 
{
    return success;
}
```

**Not:**
```objc
if (!error)
    return success;
```

or

```objc
if (!error) return success;
```

### Ternary Operator

The Ternary operator, ? , should only be used when it increases clarity or code neatness. A single condition is *all* that should be evaluated. Multiple conditions should always use if statements.

**For example:**
```objc
result = a > b ? x : y;
```

**Not:**
```objc
result = a > b ? x = c > d ? c : d : y;
```

## Error handling

When methods return an error parameter by reference, switch on the returned value, not the error variable.

**For example:**
```objc
NSError *error;
if (![self trySomethingWithError:&error]) 
{
    // Handle Error
}
```

**Not:**
```objc
NSError *error;
[self trySomethingWithError:&error];
if (error) 
{
    // Handle Error
}
```

Some of Apple’s APIs write garbage values to the error parameter (if non-NULL) in successful cases, so switching on the error can cause false negatives (and subsequently crash).

## Methods

In method signatures, there should be a space after the scope (-/+ symbol). There should be a space between the method segments.

**For Example**:
```objc
- (void)setExampleText:(NSString *)text image:(UIImage *)image;
```

**Not:**
```objc
-(void)setExampleText:(NSString *)text image:(UIImage *)image;
```

**Or:**
```objc
- (void) setExampleText:(NSString *)text image:(UIImage *)image;
```

## Variables

Variables should be named as descriptively as possible. Single letter variable names should be avoided except in `for()` loops.

Asterisks indicating pointers belong with the variable, e.g., `NSString *text` not `NSString* text` or `NSString * text`, except in the case of constants, whereby `NSString * kConstantName` should be used.

Property definitions should be used in place of naked instance variables whenever possible. Direct instance variable access should be avoided except in initialiser methods (`init`, `initWithCoder:`, etc…), `dealloc` methods and within custom setters and getters. For more information on using Accessor Methods in Initialiser Methods and dealloc, see [here](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmPractical.html#//apple_ref/doc/uid/TP40004447-SW6).

**For example:**

```objc
@interface HNCPost: NSObject

@property (nonatomic) NSString *title;

@end
```

**Not:**

```objc
@interface HNCPost : NSObject {
    NSString *title;
}
```

## Naming

Apple naming conventions should be adhered to wherever possible, especially those related to [memory management rules](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html) ([NARC](http://stackoverflow.com/a/2865194/340508)).

Long, descriptive method and variable names are good.

**For example:**

```objc
UIButton *settingsButton;
```

**Not**

```objc
UIButton *setBut;
```

A three letter prefix (e.g. `DLT`) should always be used for class names and constants, however may be omitted for Core Data entity names. Constants should be camel-case with all words capitalised and prefixed by the related class name for clarity.

**For example:**

```objc
static const NSString * HNCBasePathForAPI = @"http://apiurl.com/api/v1/";
```

**Not:**

```objc
static const NSString * basePath = @"http://apiurl.com/api/v1/";
```

Properties and local variables should be camel-case with the leading word being lowercase. 

Instance variables should be camel-case with the leading word being lowercase, and should be prefixed with an underscore. This is consistent with instance variables synthesised automatically by LLVM. **If LLVM can synthesise the variable automatically, then let it.**

**For example:**

```objc
@synthesize descriptiveVariableName = _descriptiveVariableName;
```

**Not:**

```objc
id varnm;
```

## Comments

When they are needed, comments should be used to explain **why** a particular piece of code does something.

Block comments should generally be avoided, as code should be as self-documenting as possible, with only the need for intermittent, few-line explanations.

## init and dealloc

`init` should be placed at the top of an implementation. `dealloc` methods should be placed at the bottom of the implementation directly above the `@end`

`init` methods should be structured like this:

```objc
- (instancetype)init 
{
    self = [super init]; 
    if (self) {
        // Custom initialisation
    }

    return self;
}
```

## Literals

`NSString`, `NSDictionary`, `NSArray`, and `NSNumber` literals should be used whenever creating immutable instances of those objects. Pay special care that `nil` values not be passed into `NSArray` and `NSDictionary` literals, as this will cause a crash.

**For example:**

```objc
NSArray *names = @[@"Daniel", @"Ross", @"Will", @"Marco"];
NSDictionary *ages = @{@"Daniel" : @17, @"Ross" : @15, @"Will" : @16};
NSNumber *yearOfBirth = @1996;
```

## Constants

Constants are preferred over in-line string literals or numbers, as they allow for easy reproduction of commonly used variables and can be quickly changed without the need for find and replace. Constants should be declared as `static` constants and not `#define`s unless explicitly being used as a macro.

**For example:**

```objc
static NSString * const HNCBasePathForAPI = @"http://path/to/api";

static const CGFloat HNCImagePreviewHeight = 50.0;
```

**Not:**

```objc
#define APIBasePath @"http://path/to/api"

#define previewHeight 50
```

## Enumerated Types

When using `enum`s, it is recommended to use the new fixed underlying type specification because it has stronger type checking and code completion. The SDK now includes a macro to facilitate and encourage use of fixed underlying types — `NS_ENUM()`

**Example:**

```objc
typedef NS_ENUM(NSInteger, HNCPostType) {
    HNCPostTypeLinkedPage,
    HNCPostTypeWrittenComment
};
```

## Private Properties

Private properties should be declared in class extensions (anonymous categories) in the implementation file of a class. 

**For example:**

```objc
@interface DLTAdView ()

@property (nonatomic, strong) GADBannerView *googleAdView;
@property (nonatomic, strong) ADBannerView *iAdView;
@property (nonatomic, strong) UIWebView *adXWebView;

@end
```

## Image Naming

Image names should be named consistently to preserve organisation and developer sanity. They should be named as one camel case string with a description of their purpose, followed by the un-prefixed name of the class or property they are customising (if there is one), followed by a further description of color and/or placement, and finally their state. Xcode Assets should be used wherever possible.

**For example:**

* `RefreshBarButtonItem` / `RefreshBarButtonItem@2x` and `RefreshBarButtonItemSelected` / `RefreshBarButtonItemSelected@2x`
* `ArticleNavigationBarWhite` / `ArticleNavigationBarWhite@2x` and `ArticleNavigationBarBlackSelected` / `ArticleNavigationBarBlackSelected@2x`.

## Booleans

Since `nil` resolves to `NO` it is unnecessary to compare it in conditions. Never compare something directly to `YES`, because `YES` is defined to 1 and a `BOOL` can be up to 8 bits.

This allows for more consistency across files and greater visual clarity.

**For example:**

```objc
if (!someObject) 
{
// Code
}
```

**Not:**

```objc
if (someObject == nil) 
{
// Code
}
```

-----

**For a `BOOL`, here are two examples:**

```objc
if (isAwesome)
if (![someObject boolValue])
```

**Not:**

```objc
if ([someObject boolValue] == NO)
if (isAwesome == YES) // Never do this.
```

-----

If the name of a `BOOL` property is expressed as an adjective, the property can omit the “is” prefix but specifies the conventional name for the get accessor, for example:

```objc
@property (assign, getter=isEditable) BOOL editable;
```
Text and example taken from the [Cocoa Naming Guidelines](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingIvarsAndTypes.html#//apple_ref/doc/uid/20001284-BAJGIIJE).

## Singletons

Singleton objects should use a thread-safe pattern for creating their shared instance.
```objc
+ (instancetype)sharedInstance 
{
   static id sharedInstance = nil;

   static dispatch_once_t onceToken;
   dispatch_once(&onceToken, ^{
      sharedInstance = [[self alloc] init];
   });

   return sharedInstance;
}
```
This will prevent [possible and sometimes prolific crashes](http://cocoasamurai.blogspot.com/2011/04/singletons-your-doing-them-wrong.html).

## Xcode project

The physical files should be kept in sync with the Xcode project files in order to avoid file sprawl. Any Xcode groups created should be reflected by folders in the filesystem, this is because it's much easier to then navigate outside of Xcode.

# Other Objective-C Style Guides

If mine doesn't fit your tastes, have a look at some other style guides:

* [NYTimes](https://github.com/NYTimes/objective-c-style-guide)
* [Google](http://google-styleguide.googlecode.com/svn/trunk/objcguide.xml)
* [GitHub](https://github.com/github/objective-c-conventions)
* [Adium](https://trac.adium.im/wiki/CodingStyle)
* [Sam Soffes](https://gist.github.com/soffes/812796)
* [CocoaDevCentral](http://cocoadevcentral.com/articles/000082.php)
* [Luke Redpath](http://lukeredpath.co.uk/blog/my-objective-c-style-guide.html)
* [Marcus Zarra](http://www.cimgf.com/zds-code-style-guide/)
