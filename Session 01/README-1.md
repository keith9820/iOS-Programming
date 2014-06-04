#Chapter 1 - A Simple iOS Application

### Course Introduction


We are developing native applications for the Apple mobile devices:

	iPhone, iPad, iTouch

Apple offers 2 developer programs:

*  Mac developer
*  iOS developer


Join the Apple developer program, iOS developer registration is $99 and allows you to run applications on a physical device and deploy to the app store (sign code)

iOS is the operating system for mobile devices

Both developer programs offer tools (Xcode) and the Cocoa framework
iOS developers use Cocoa framework for iOS

Cocoa framework consists of other frameworks (libraries)


Open an empty workspace and explain areas of the Xcode IDE

### Chapter 1 - A Simple iOS Application
````page 1````

This chapter is about going through the motions
Mimicry is a powerful form of learning, don’t worry if things don’t make sense yet

When writing applications, you need to answer 2 questions: 
 How do I create the visual layout (User Interface) that I want
 How do I deal with user interaction (i.e.  run code when the user taps a button)


When an iOS application starts, it puts a view on to the screen.  Think of a view as a background, or canvas, on which everything else appears (i.e. buttons, labels, maps, etc...)

The iOS SDK is an object-oriented library, and views are represented by objects

Each visual element is a view

Each view is an object of type UIView or one of its subclasses

We will discuss objects and subclassing later

For the first application, we will visually create and configure view objects.  

The Quiz application will show a question then reveal the answer when a button is pressed
Pressing another button will show the next question

Show Application

### Creating an Xcode project

Step 1:  Create a Single View Application named “Quiz”     	    Product Name: Quiz    Company Identifier: edu.nyu.scps  	    Class Prefix: BNR 			Device: iPhone
page 2

Even though we chose iPhone as the device, this app will still run on iPad though it won’t make best use of the screen space.  That’s okay for now.  

In the beginning we will stick to the iPhone device template to learn the fundamentals of iOS development.  Later, we’ll look at some iPad-only options and how to make apps run on both.


### Building interfaces
````page 5````

To create the app’s user interface, we’ll use Xcode (formerly Interface Builder)
GUI builder is an object editor:  You create view objects and save them into an archive

The archive is an XML representation of archived objects, called a XIB file.

When you build the project, the XIB file is compiled into a binary NIB file 

Developers use XIB files (easier to parse)
The application uses NIB files (smaller and easier to parse).

XIB and NIB are used interchangeably.

When an app is built, the compiled NIB file is copied into the application’s bundle.
(Bundle is a directory containing the application’s executable and any resources it uses)

When your application reads in (loads) the NIB file from the bundle at runtime, the objects in the archive are brought to life.

Complex applications can have many NIB files, ours has only 1

Open QuizViewController.xib, notice canvas and doc view
Doc view becomes document outline by clicking the disclosure button
The Document Outline shows:

 File’s Owner - The object that has access to the archived objects in the XIB file.  This object is responsible for managing events that occur on the interface. 
  First Responder - Relic from Cocoa for Mac OSX, ignore. 
  View - an instance of UIView, represents the application interface.

The canvas portion of the editor area is for viewing and manipulating the layout of your interface.

----
Step 2:  Create the visual interface.  Add elements from the utilities area library section

----

Use the attributes inspector to set instance variables of selected object


### Model-View-Controller
````page 9````

The Model-View-Controller pattern provides that each object of your application be either a view object, model object, or controller object.

View objects are visible to the user.  

The labels are object instances of type UILabel
The buttons are object instances of type UIButton

Model objects contain data and have no concept of a user interface.  Our model objects are going to be standard collection classes to hold the questions and answers.

View and Model objects are the factory workers of an application, they focus on specific tasks.
i.e.  UILabel knows how to display text in a given font, within the bounds of a rectangle and an NSString instance knows how to store characters as a string.  

UILabel doesn’t know what text it’s going to display
NSString doesn’t know what characters it will have.

The controller object comes in to manage these objects within the application.

Controller objects control the flow within an application and do things like load data from a database, save data to the file system, handle actions, etc...

Controllers tend to be application specific -- least reusable of these objects

The project template created a controller for us named QuizViewControlller.  In our app, the controller will be responsible for showing a new question when the first button is tapped, and showing the answer when the second button is tapped


### Declarations
````page 11````

To manage the relationships and responsibilities, we need to give QuizViewController some properties and 2 methods.

    Questions
    Answers
    currentQuestionIndex
    questionField  (pointer to UILabel)
    answerField  (pointer to UILabel)
    
    @interface BNRQuizViewController ()
    
    @property (nonatomic, weak) IBOutlet UILabel *questionLabel;
    @property (nonatomic, weak) IBOutlet UILabel *answerLabel;
    
    @property (nonatomic) int currentQuestionIndex;
    @property (nonatomic, copy) NSArray *questions;
    @property (nonatomic, copy) NSArray *answers;
    
    @end

### Declaring methods
````page 12````

Each button needs to trigger a method to run when tapped.  In this case, we don’t need to forward-declare the function signature in the header file.

    - (IBAction)showQuestion:(id)sender;
    - (IBAction)showAnswer:(id)sender;

IBAction and IBOutlet allow you to connect your controller to your view objects in the XIB file


### Making connections
````page 13````

A connection is used to relate an iVar in the controller to a UIView instance in the XIB file.
These iVars in the controller have a pointer to the object in the XIB file

### Setting pointers
````page 14````

Visually create the connections in IB

Select the “File’s Owner” and notice in the identity inspector that the class is set to QuizViewController.

Drag from “Files Owner” to UILabel.   Control-Drag or right-click Drag

Notice that only fields decorated with IBOutlet are shown


### Setting targets and actions
````page 15````

When a UIButton is tapped, it sends a message (the action) to another object (the target).


Open connections inspector and drag from Touch Up Inside event to “Files Owner”
or control-drag from button to “Files Owner”

Notice that only methods decorated with UIAction are shown

### Implementing methods
````page 17````

The showQuestion and showAnswer methods were only declared in the header file, we need to implement them in the .m file. 

Delete boilerplate code.

When the application launches, the QuizViewController will be sent the message initWithNibName:bundle:.  Implement this method to load the questions and answers.


    - (id)initWithNibName:(NSString *)nibNameOrNil bundle:(NSBundle *)nibBundleOrNil
    {
        //call superclass init method
        self = [super initWithNibName:nibNameOrNil bundle:nibBundleOrNil];
        if (self){

    
            self.questions = @[@"From what is Cognac made?",
                           @"What is 7+7?",
                           @"What is the capital of Vermont?"];
        
            self.answers = @[@"Grapes", @"14", @"Montpelier"];
            self.currentQuestionIndex = -1;
        }
        return self;
    }


Now that the initialization is taken care of, let’s implement the action methods.

    - (IBAction)showQuestion:(id)sender{
    
        self.currentQuestionIndex++;
        if(self.currentQuestionIndex == self.questions.count){
            self.currentQuestionIndex = 0;
        }
    
        self.questionLabel.text = self.questions[self.currentQuestionIndex];
   
        //clear answer
        self.answerLabel.text = @"???";
    }

    - (IBAction)showAnswer:(id)sender{
    
        self.answerLabel.text = self.answers[self.currentQuestionIndex];
    }
    
**BUILD AND RUN**

