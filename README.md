These guidelines build on Apple's existing [Coding Guidelines for Cocoa](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html).
Unless explicitly contradicted below, assume that all of Apple's guidelines apply as well.

## Whitespace

 * Spaces, not tabs.
 * Tab width of 4.
 * End files with a newline.
 * Make liberal use of vertical whitespace to divide code into logical chunks, but no more than one newline between each chunk.
 * Don’t leave trailing whitespace.
    * Not even leading indentation on blank lines.

## Documentation and Organization

 * All public method declarations should be documented.
 * Comments should be [Doxygen](http://www.stack.nl/~dimitri/doxygen/)-style.
 * Document whether object parameters allow `nil` as a value.
 * Comments in code should be used to describe what a logical chunk does. Use your judgement and add comments where you think something needs explaining. In other words, do not explain what each line of code does, but give a high level description of what a logical chunk of code does if you think it is needed.
 * Use `#pragma mark`s to categorize methods into functional groupings and protocol implementations, following this general structure:

```objc
#pragma mark - Properties

@dynamic someProperty;

- (void)setCustomProperty:(id)value {}

#pragma mark - Lifecycle

+ (instancetype)objectWithThing:(id)thing {}
- (instancetype)init {}

#pragma mark - Drawing

- (void)drawRect:(CGRect) {}

#pragma mark - Another functional grouping

#pragma mark GHSuperclass

- (void)someOverriddenMethod {}

#pragma mark - NSCopying

- (id)copyWithZone:(NSZone *)zone {}

#pragma mark - NSObject

- (NSString *)description {}

#pragma mark - Helpers
```

## Declarations

 * Always declare memory-management semantics on every property.
 * Declare properties `readonly` if they are only set once in `-init`.
 * Don't use `@synthesize` unless the compiler requires it. Note that optional properties in protocols must be explicitly synthesized in order to exist.
 * Avoid `weak` properties whenever possible. A long-lived weak reference is usually a code smell that should be refactored out.
 * Instance variables should be prefixed with an underscore (just like when implicitly synthesized).
 * Constructors should generally return [`instancetype`](http://clang.llvm.org/docs/LanguageExtensions.html#related-result-types) rather than `id`.
 * Don't put a space between an object type and the protocol it conforms to.

```objc
@property (nonatomic, strong) NSObject<Protocol> *object;
```

## Expressions

 * Use dot-syntax when invoking idempotent methods, including setters and class methods (like `NSFileManager.defaultManager`).
 * Use object literals, boxed expressions, and subscripting over the older, grosser alternatives. (Use `@[thing]` instead of `[[NSArray alloc] initWith`...).
 * Use `[RBObject new]` instead of `[[RBObject alloc] init]`.
 * Comparisons should be explicit, including `BOOL`s.

```objc
if (boolean == YES) {
    // Do something
}

if (thing1 == thing2) {
    // Do something
}
```

 * Prefer positive comparisons to negative.
 * Long form ternary operators should only used for assignment and arguments.

```objc
Blah *a = (stuff == thing) ? foo : bar;
```

 * Separate binary operands with a single space, but unary operands and casts with none:

```c
void *ptr = &value + 10 * 3;
NewType a = (NewType)b;

for (int i = 0; i < 10; i++) {
    doCoolThings();
}
```

## Control Structures

 * Always surround control structure bodies with curly braces.
 * Starting curly braces should begin on the same line as the control structure and closing braces should be on a new line.
 * `else` or `else if` control structures should be on the same line as the previous ending curly brace.
 * Put a single space after keywords and before their parentheses.
 * No spaces between parentheses and their contents.
 * Return and break early.

```objc
if (somethingIsBad == YES) {
    return;
}

if (something == nil) {
    // do stuff
} else {
    // do other stuff
}
```

## Exceptions and Error Handling

 * Don't use exceptions for flow control.
 * Use exceptions only to indicate programmer error.

## Blocks

 * Blocks should have a space between their return type and name.
 * Block definitions should omit their return type when possible.
 * Block definitions should omit their arguments if they are `void`.
 * Parameters in block types should be named.

```objc
void (^blockName1)() = ^{
    // do some things
};

id (^blockName2)(id) = ^id (id args) {
    // do some things
};
```

## Literals

 * Avoid making numbers a specific type unless necessary (for example, prefer `5` to `5.0`).
 * Dictionary literals should have no space between the key and the colon, and a single space between colon and value.

``` objc
NSArray *theStuff = @[@"One", @"Two", @"Three"];

NSDictionary *keyedStuff = @{GHDidCreateStyleGuide: @YES, RBDidChangeCodeStyle: @YES};
```

 * Longer or more complex literals should be split over multiple lines:

``` objc
NSArray *theStuff = @[
    @"Got some long string objects in here.",
    [AndSomeModelObjects too],
    @"Moar strings."
];

NSDictionary *keyedStuff = @{
    @"this.key": @"corresponds to this value",
    @"otherKey": @"remoteData.payload",
    @"some": @"more",
    @"JSON": @"keys",
    @"and": @"stuff"
};
```

## Categories

 * Categories should be named for the sort of functionality they provide. Don't create umbrella categories.
 * Category methods should always be prefixed.
 * If you need to expose private methods for subclasses or unit testing, create a class extension named `Class+Private`. Some older projects use the `Class+ForSubclassEyesOnly` naming.
