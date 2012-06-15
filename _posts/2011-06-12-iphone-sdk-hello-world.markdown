--- 
layout: post
title: IPhone SDK Hello World
wordpress_id: 80
wordpress_url: http://www.techtraits.ca/?p=80
date: 2011-06-12 18:44:32 +00:00
---
<hr />
<p style="text-align: justify;">This is a tutorial for beginners to the IPhone SDK and includes instructions on creating a basic HelloWorld program. The program demonstrates the use of Views, Labels and UIButtons. I will be using the 4.3 SDK so some features may not be available in the earlier version.</p>
<!--more-->
<h3 style="text-align: justify;"><a href="http://www.techtraits.ca/wp-content/uploads/2011/06/projecttype.png"><img class="alignright size-full wp-image-84" title="projecttype" src="http://www.techtraits.ca/wp-content/uploads/2011/06/projecttype.png" alt="" width="500" height="340" /></a></h3>
<h3>Source Code</h3>
<a href="http://www.techtraits.ca/wp-content/uploads/2011/06/HelloWorld.zip">HelloWorld Source Project</a>
<p style="text-align: justify;"></p>

<h3 style="text-align: justify;">Step 1: Download XCode and the SDK</h3>
<p style="text-align: justify;">The first thing we need to do is to download the development tools namely <a href="http://developer.apple.com/tools/xcode/">XCode</a> and the <a href="http://developer.apple.com/devcenter/ios">SDK</a>. To download either of these you will need to register for the Apple ID and Apple developer Connection (ADC). Install both these tools, the procedure to do so is fairly trivial so I am not going into details.</p>

<h3 style="text-align: justify;">Step 2: Create a new Project</h3>
<p style="text-align: justify;">Now we have the tools we need to start coding but before we do we need to create a new project. <strong>Select File &gt; New Project</strong> or <strong>Apple Key + Shift + N</strong> to bring up the new project menu. Select the <strong>Applications </strong>item of the <strong>IPhone OS</strong> section from the menu on the left, and select <strong>View Based Application </strong>from the icons on the right. When prompted enter a project name, I have used HelloWorld in the sample code.</p>
<p style="text-align: justify;"></p>

<h3>Step: 3 Update the settings</h3>
<p style="text-align: justify;">Once you compete the wizard creates the project you will be taken to the main settings page. Here you can setup the device orientation settings and the application icon. (By dragging a properly sized png file into the correct box).</p>
<p style="text-align: justify;">When you create a project you will see a window similar to the one shown in the image on the right. There are four source files in the<strong> </strong>package; <a href="http://www.techtraits.ca/wp-content/uploads/2011/06/menu.png"><img class="alignright size-full wp-image-88" title="menu" src="http://www.techtraits.ca/wp-content/uploads/2011/06/menu.png" alt="" width="260" height="239" /></a>
HelloWorldAppDelegate.h
HelloWorldAppDelegate.m
HelloWorldViewController.h
HelloWorldViewController.m</p>
I am ignoring the xib files for now because for this tutorial I will be adding the interface elements directly and not through the interface builder. I would like to highlight some important parts of the auto-generated code so open the HelloWorldAppDelegate.h file and note that it contains a reference to a UIWindow object (Line 17) which controls all the user interaction for this application and will manage all other interface components. It also contains a reference to HelloWorldViewController (Line 19) which we will see later manages our one sample view.

In the source file for the application delegate (HelloWorldAppDelegate.m) we find an auto generated <strong>applicationDidFinishLaunching</strong> method. This, as the name suggests, invoked when the application has been loaded and in this method we will add our HelloWorldViewController object to the UIWindow and make it visible.
<pre lang="objc">self.window.rootViewController = self.viewController;
[self.window makeKeyAndVisible];</pre>
The IPhone SDK UI follows the <a href="http://en.wikipedia.org/wiki/Model-view-controller">Model View Controller (MVC)</a> design pattern and hence each <em>view </em>has a corrsponding View Controller Object. XCode has automatically created a view for us, (its defined in the HelloWorldViewController.xib file you can see it by double clicking it in the side menu) and linked it to HelloWorldViewController. We now need to add controls to the view handle the generated events. Note we can add controls via a GUI interface called the Interface Builder but I am using the programmatic way for this tutorial.

<a name="Step_3(3A)_Adding_Controls_and_Handler_to_header_file"></a>
<h3>Step 3: Adding Controls and Handler to header file</h3>
Open the HelloWorldViewController.h file and add declarations for a button, a label and a view within the curly braces delimiting the HelloWorldViewController interface declaration add the following lines.
<pre lang="objc">IBOutlet UIButton *button;
IBOutlet UILabel *label;
IBOutlet UIView *myView;</pre>
We are defining a view to display a button and label to go in the view. After the curly braces add a method declaration to accept the click event of the button and also properties to access the  UI elements; myButton, myLabel and myView. Also add properties for these  Your entire code should look something like this:
<h3><a href="http://www.techtraits.ca/wp-content/uploads/2011/06/mainsettings.png"><img class="alignright size-full wp-image-85" title="mainsettings" src="http://www.techtraits.ca/wp-content/uploads/2011/06/mainsettings.png" alt="" width="531" height="730" /></a></h3>
<pre lang="objc">//
//  HelloWorldViewController.h
//  HelloWorld
//

#import <UIKit/UIKit.h>

@interface HelloWorldViewController : UIViewController {
 IBOutlet UIButton *button;
 IBOutlet UILabel *label;
 IBOutlet UIView *myView;   
}
 -(IBAction)handleEvent:(id)sender;
@property (nonatomic, retain) IBOutlet UIButton *button;
@property (nonatomic, retain) IBOutlet UILabel *label;
@property (nonatomic, retain) IBOutlet UIView *myView;

@end</pre>
<h3>Step 4: Adding Controls</h3>
Add a <strong>synthesize</strong> call for each of the three UI elements to create the getter and setters after the @implementation HelloWorldViewController line in the HelloWorldViewController.m file as shown below.
<pre lang="objc">@synthesize button;
@synthesize label;
@synthesize myView;</pre>
Now find the comment saying "Implement loadView if you want to create a view hierarchy programmatically." and un-comment the loadView method that follows this line. Add the following lines to the method to create a button and label;
<pre lang="objc">@synthesize button;
- (void)loadView
{
    // create and configure the view
    CGRect cgRct = CGRectMake(0.0, 0.0, 480, 320); //define size and position of view
    myView = [[UIView alloc] initWithFrame:cgRct]; //initialize the view
    myView.autoresizesSubviews = YES;              //allow it to tweak size of elements in view
    self.view = myView;

    // create a UIButton (UIButtonTypeRoundedRect) and play arround with settings
    button = [UIButton buttonWithType:UIButtonTypeRoundedRect];
    button.frame = CGRectMake(100, 100, 100, 50); // size and position of button
    [button setTitle:@"Add" forState:UIControlStateNormal];
    button.backgroundColor = [UIColor clearColor];
    button.adjustsImageWhenHighlighted = YES;

    //Add action handler and set current class as target
    [button addTarget:self action:@selector(action:) forControlEvents:UIControlEventTouchUpInside];

    //Display Button
    [self.view addSubview:button];

    //create a label
    cgRct = CGRectMake(100, 170, 100, 50); //define size and position of label
    label = [[UILabel alloc] initWithFrame:cgRct];
    label.text = @"Hello World";

    //Display Label
    [self.view addSubview:label];
}</pre>
Click the <strong>Run</strong> button to check your interface is drawn correctly It should look something like to screen shot below.

<a href="http://www.techtraits.ca/wp-content/uploads/2011/06/button.png"><img class="alignleft size-full wp-image-98" title="button" src="http://www.techtraits.ca/wp-content/uploads/2011/06/button.png" alt="" width="264" height="73" /></a><a href="http://www.techtraits.ca/wp-content/uploads/2011/06/phone.png"><img class="aligncenter size-medium wp-image-99" title="phone" src="http://www.techtraits.ca/wp-content/uploads/2011/06/phone-163x300.png" alt="" width="195" height="358" /></a>

&nbsp;
<h3>Step 5: Handling Actions</h3>
We have already told the button to send an event (action) to the current class (self) in case of an <strong>UIControlEventTouchUpInside </strong>event but there is no handler defined to accept the event. We define a handler method in the class by adding the method shown below. It is doing two things, changing the text property of the label and adding a message to the log file.
<pre lang="objc">-(void)action:(id)sender
{
    NSLog(@"UIButton was clicked");
    label.text = @"Bye World";
}</pre>
Now click the Build and Go button again and click the button. The text of the label should change.