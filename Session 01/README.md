# C Language Refresher
----

### Structs

Used to package data together.  
Like classes in OO programming, but without methods.

Declared in the struct namespace, so the `struct` keyword is required when declaring variables.

declare:

````
struct SIZE{
  height float,
  width  float
};
````

use:

````
  struct SIZE box;
  
  box.height = 100;
  box.width = 100;
  
````


#### Enums

Used to assign labels to integral constants.

Declared in the enum namespace, so the `enum` keyword is required when declaring variables.

declare:


````
enum DIRECTION {
  north,
  south,
  east,
  west
};
````


````
enum {
  NSOrderedAscending = -1,
  NSOrderedSame,
  NSOrderedDescending
};
````

use:

````
  enum DIRECTION heading = south;
  
  if (heading == north)
    //do something
    
  int sort_order = NSOrderedDescending;
  
````


#### typedef

Used to form complex types from more basic ones and assign simpler names to existing types:

````
  typedef int NSInteger;
  
  NSInteger x = 5;    
  
````  

typedef types can be combined:

````
  typedef NSInteger NSComparisonResult;
  
  NSComparisonResult r = NSOrderedSame;  //0    
  
````  

we can now make structs simpler:

````
  typedef struct SIZE{
    height float,
    width  float
  } SIZE;

  SIZE box;
  
  box.height = 100;
  box.width = 100;
````

#### Pointers

The pointer data type stores a memory address.

It is declared as <type> *_&lt;variable name>_

These are both acceptable:

    int *x;
    int* x;
    int*x;


but the cool kids put a space before the * so that there's no mistake when declaring multiple variables at once:
  
    int* x, y; // x is a pointer, y is an int;
    int *x, y; // same;
    int *x, *y; // x and y are both pointers to an int;

A typedef can make things easier to read:

    typedef int* IntPtr;
    IntPtr a;
    
    typedef char* String;
    String name = "keith";
    
#### The Preprocessor

Runs before the compiler to manipulate your source code.  Preprocessor directives start with a # and must be the first non-space character on a line.

The \#define statement allows you to assign labels to constants.

    #define PI 3.141592654;
    
    #define TWO_PI 2.0 * PI;

----

# Cocoa Touch Concepts

#### Size
	
* height
* width

#### Point
 
*  x
*  y




#### Rectagle

#### Bounds


