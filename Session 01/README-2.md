### Chapter 2 - Objective-C
````page 29````

iOS applications are written in Objective-C language using the libraries found in  the Cocoa for iOS framework

Objective-C is a simple extension of the C language

### Objects

Object-Oriented programming is a software development paradigm that creates software models (data structures) that correspond to real-world objects.

Let’s image a party.  A party has a few attributes that are specific such as:

*  Name
*  Date
*  Budget
*  List of invitees

Our party can also do things such as:

* Send email reminders to the invitees
* Print name tags
* Cancel the party

In traditional C, you would define a structure to hold the party details and separate C functions to perform the actions.

In Objective-C, you create classes which are data structures containing the party attributes as well as the action functions.

A class is like a template from which you create class instances known as objects.
Each objects has its own data.

The attributes are called fields and the functions are called methods.

The objects fields comprise its internal state and this data is not available outside the class instance.  The methods have access to the fields and the field values can be made available outside the class via property methods.  “properties” for short.

If you want an object to execute one of its methods, you pass a message to the object

    @interface Party : NSObject
    {
        NSString *name;
        NSDate *date;
        int budget;
        NSMutableArray *invitees;
    }
    - (void)sendReminder;
    - (void)cancel;
    - (void)addAttendee:(NSString *)person;
    - (void)addAttendee:(NSString *)person withDish:(NSString *)dish;
    @end

### Using Instances
````page 30````

To create an instance of a class, you declare a pointer of the class type and initialize it using alloc/init (or new)

A pointer variable stores the location of an object in memory, not the object itself:

    Party *partyInstance;

Creating objects

An object has a life span:  it is created, sent messages, then destroyed.

To create an object, you send the class an alloc message.  

The alloc method creates a chunk of memory for the objects and returns a pointer to it.

You can’t use an object created in this way until you send the init message.

    partyInstance = [partyInstance init];

Although alloc returns an object, it isn’t valid until it’s been initialized.

Since an object cannot be used until it’s been allocated and initialized, commonly written as:

    Party *party = [[Party alloc] init];  //nested message send
    
-or-

    Party *party = [Party new]; 
    
### Sending messages
````page 31````

After an object has been created you send it messages.

Let’s look at anatomy of message.

A message is always contained in square brackets [ ]

It has 3 parts:

receiver 	pointer to the object being sent the message 
selector 	the name of the message to execute 
arguments 	the values being supplied as parameters

A party might have a list of attendees that you can add to by sending the party the message addAttendee:

    [partyInstance addAttendee:@”Bob”];

methods can take a single argument, or none (such as init), or many arguments.

When a method accepts multiple arguments, each argument (after the first) is paired with a label.

The label ends with a colon.


In Objective-C the name of a method must be unique.
The name of the method includes the labels, so the method name is addAttendee:withDish: including the colons.

### Destroying objects
````page 32````

To destroy an object, set the variable that points to it to nil
nil is the zero-pointer (NULL in C)

nil means an object hasn’t been set yet.
Could say 

    if (partyInstance != nill)
    
or shorthand

    if (!partyInstance)  //since nil == false

If you send a message to a nil object, nothing happens.  No error


### Beginning RandomItems
````page 33````

Before diving into the UIKit library and iOS applications, let’s look at Objective-C using a simple command line application.

____
**Step 1:**  Create a command-line tool named RandomPossessions

**Step 2:**  Open main.m, show NSMutableArray

    @autoreleasepool {
        
        NSMutableArray *items = [[NSMutableArray alloc]init];
        
        [items addObject:@"One"];
        [items addObject:@"Two"];
        [items addObject:@"Three"];
        
        //send another message
        [items insertObject:@"Zero" atIndex:0];
        
        for (int i=0; i<items.count; i++){
            NSLog(@"%@", [items objectAtIndex:i]);
        }
        items = nil;
    }

    Run and show in console.  Show inserting breakpoint

### Creating strings
````page 36````

In Objective-C, string are NSString instances.  Use @”xxx” to denote a NSString instance.  The @ syntax is a convenience for creating (alloc/init) strings


### Format strings
````page 37````

Look at the NSLog function to write to the debug output window

From C, we’re already familiar with format strings containing format tokens

    %d - integer
    %f - float
    %c - char
    %@ is for Obj-C it sends the description message to an object and returns the result

Every object implements the description method (it’s inherited from NSObject)


### NSArray and NSMutableArray
````page 37````

An array is a collection of objects.  Cocoa provides others including NSDictionary, NSSet, etc...

An array is a set of objects accessed at an index

NSArray is immutable which means you cannot add or remove objects after initialization

NSMutableArray is the mutable subclass which allows dynamic addition/deletion

Array holds objects so it actually only contains pointers to those objects

Array has a count property to indicate the number of items.

You cannot add C primitive types to an NSArray, only objects.  But you can use the classes which represent primitive types such as NSInteger

You cannot add nil to NSArray.  nil indicates the end of the array internally.  Use NSNull.

### Subclassing an Objective-C class
````page 38````

Classes such as NSMutableArray exist in a hierarchy.  Every class has exactly one superclass except the root class which is NSObject

Every class inherits methods and instance variables (iVars) defined in NSObject

alloc, init, description all come from NSObject

A subclass adds methods and iVars to an existing class to extend its behavior

i.e.  NSMutableArray extends NSArray

A method can also override methods of a superclass (description)

### Creating an NSObject subclass
````page 39````

Here we’re going to create a class that extends NSObject.  BNRItem will represent an item a person owns in the real world.

----
##### Step 3:  Create BNRItem class

----

A class consists of 2 files:

 An interface or header file (.h)
 An implementation file (.m)


### Instance variables
````page 42````

Instance variables (iVars) represent an object’s internal state.

iVars are created between curly braces { }, traditionally in the .h file (2.0 use .m file)

    @interface BNRItem : NSObject
    {
        NSString *itemName;
        NSString *serialNumber;
        int valueInDollars;
        NSDate *dateCreated;
    }
    @end

### Accessor methods
````page 43````

iVars are not accessible outside the class.  To set them or retrieve their values, use accessor methods (accessors or properties).

Individually called getters and setters

By convention, getter method name is same as iVar,
setter prepends set then capitalizes the iVar

----
##### Step 4:  Provide declaration and implementation for all iVar accessors

----
dateCreated is read-only

    #import header file.  import is same as include except a file is sure to be included only once

In **main.m**, create an instance of BNRItem and print it out:

        BNRItem *p = [BNRItem new];
        NSLog(@"%@  %@  %@  %d"
                    , [p itemName]
                    , [p dateCreated]
                    , [p serialNumber]
                    , [p valueInDollars]);

Look at console window and see 3 NULL strings and a 0.  When an object is created, all it fields are initialized to default values.

Set p’s values and log again

        [item setItemName:@"Red Sofa"];
        [items addObject:item];
        
        BNRItem *p = [BNRItem new];
        [p setItemName:@"Red Stapler"];
        [p setSerialNumber:@"123456"];
        [p setValueInDollars:15];

----

##### Step 5:  Override description

----

        - (NSString *)description{
                NSString *desc = [[NSString alloc] 
				    initWithFormat:@"%@ (%@): Worth $%d, recorded on %@",
                                            itemName,
                                            serialNumber,
                                            valueInDollars,
                                            dateCreated];
        return desc;
    }




### Initializers
````page 47````

As you start to write more complicated classes, you will want to create your own initializer methods.
Like init, but with arguments to provide the initial values.

BNRItem would be much cleaner if we could pass in one or more instance variables as part of the initialization process

Many classes have one or more initializers, all starting with init (by convention).

Regardless of how many initialization methods a class has, only one is the designated initializer.

The designated initializer typically has parameters for the most important and frequently used instance variables of an object.

BNRItem has 4 iVars but only 3 are writeable, therefor BNRItem’s designated initializer should accept three arguments:

    - (id)initWithItemName:(NSString *)name valueInDollars:(int)value serialNumber:(NSString *)sn{

        self = [super init];
        if (self){
            [self setItemName:name];
            [self setValueInDollars:valueInDollars];
            [self setSerialNumber:serialNumber];
        
            dateCreated = [[NSDate alloc] init];
        }
        return self;
    }
    
*Initializers always return id*

id is a “pointer to any object”.  It would be wrong to return BNRItem because if you subclassed it, you couldn’t return the subclass, always BNRItem.  You cannot override a method to change its return type.

### Self
````page 49````

Inside a method, self is an implicit local variable that automatically points to the object that was sent the message.

### super
````page 50````

When overriding a method, you typically want to refer to parent class


### Other initializers and the initializer chain
````page 51````

A class can have more than one initializer, but to prevent errors and make you code more maintainable, you should chain them together.  All other initializers call the default initializer with default values.

Some simple rules for initializers:
 
1.  A class inherits all initializers from its superclass and can add its own
2.  Each class picks one initializer as the designated initializer
3.  The designated initializer calls the superclasse’s designated initializer
4.  Any other initializer method calls the designated initializer
5.  If a class declares a designated initializer different from it’s parent, the parent’s designated initializer should be overridden to call the new designated initializer

----
Step 5:  Override init to call designated initializer with default values

Step 6:  Change code in main.m to instantiate p using the designated initializer 	     (page 52 - 53)

----
### Class Methods
````page 53````

Methods come in 2 flavors:

*  Instance methods
*  Class methods

Class methods (like alloc) are messages sent to the class itself and typically create new instances of the class or return some global property of the class.

----
Step 7:  Create static method randomItem

    + (id)randomItem
    {
    // Create an array of three adjectives
    NSArray *randomAdjectiveList = [NSArray arrayWithObjects:@"Fluffy",
                                    @"Rusty",
                                    @"Shiny", nil];
    // Create an array of three nouns
    NSArray *randomNounList = [NSArray arrayWithObjects:@"Bear",
                               @"Spork",
                               @"Mac", nil];

    NSInteger adjectiveIndex = rand() % [randomAdjectiveList count];
    NSInteger nounIndex = rand() % [randomNounList count];
    
    // Note that NSInteger is typedef to "unsigned long"
    
    NSString *randomName = [NSString stringWithFormat:@"%@ %@",
                            [randomAdjectiveList objectAtIndex:adjectiveIndex],
                            [randomNounList objectAtIndex:nounIndex]];

    int randomValue = rand() % 100;
    NSString *randomSerialNumber = [NSString stringWithFormat:@"%c%c%c%c%c",
                                    '0' + rand() % 10,
                                    'A' + rand() % 26,
                                    '0' + rand() % 10,
                                    'A' + rand() % 26,
                                    '0' + rand() % 10];

    // Once again, ignore the memory problems with this method
    BNRItem *newItem =
    [[self alloc] initWithItemName:randomName
                    valueInDollars:randomValue
                      serialNumber:randomSerialNumber];
    return newItem;
    }

### Testing your subclass
````page 55````

Create 10 new items.

Replace code in main.m.  Instead of hard-coding the item, use the new randomItem class message.

    int main (int argc, const char * argv[])
    {
        @autoreleasepool {
            // Create a mutable array object, store its address in items variable
            NSMutableArray *items = [[NSMutableArray alloc] init];

            for (int i = 0; i < 10; i++) {
                BNRItem *p = [BNRItem randomItem];
                [items addObject:p];
            }
            for (BNRItem *item in items)
                NSLog(@"%@", item);

            items = nil;      
        }
        return 0;
    }


### Exceptions and Unrecognized Selectors
````page 56````

An object only responds to a method if its class implements the associated method.  Objective-C is a dynamically-typed language, so it can’t always figure at compile time whether an object will respond to a method.  Xcode will try to help, but isn’t reliable in every situation.

If your application compiles, it can still get a run-time exception if you call an unknown message.


### Fast Enumeration
````page 57````

    for (BNRItem *item in items)
        NSLog(@"%@", item);


A closer look at Rand()

    for (int i=0; i<100; i++){
       printf("%d\n", rand());
    }

    NSString *result = @"";
    for (int i=0; i<100; i++){
        result = [NSString stringWithFormat:@"%@%d\n", result, rand()];
    }


**homework:**  

*  Create the RandomPossessions project
*  Complete the silver challenge
*  Read chapter 2
