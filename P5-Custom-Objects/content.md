---
title: Add Custom Objects Using SpriteBuilder
slug: penguins-seals
---

Seals are the bad guys in Peeved Penguins. The goal is to get rid of all
of them! To get rid of them, you shoot penguins at them.

So next, we want to create the Seal and Penguin objects in
SpriteBuilder.

Penguins
========

Let's start with the penguins. We want penguins and seals to each get
their own .ccb files because they will be mapped to different
Objective-C classes. That will allow us to distinguish between them
later on when we add the shooting mechanism to our game.

Create a new interface file (Top bar: File\>New\>File) and make sure
that you create it at the root level of the project. Choose Sprite as
the root node:

![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/Spritebuilder_Penguin.png)

Now you need to set an image for the penguin. The easiest way is to
select the root node (our CCSprite) is to select it from the timeline at
the bottom of the screen. Once you have selected the CCSprite, go to the
right pane and set the "Sprite Frame" property to the
*flyingpenguin.png* image:

![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/Spritebuilder_SetSpriteFrame.png)

Now you should see the penguin image on the center of the stage.

Later, when we implement the gameplay, we want to shoot these penguins
from our catapult. This means we want to turn these penguins into
physics objects as well so that they can interact with the other physics
bodies in the scene.

Select the penguin, open the third tab and check the box "Enable
physics". Once you check the box, you will see 4 pink dots forming a
rectangle. This shape represents the shape of your physics body:

![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/Spritebuilder_enablePhysics.png)

By default this body is a square. For our flying penguin a *circle*
seems like a better choice, so change the physics shape in the dropdown
accordingly:

![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/Spritebuilder_changePhysicsBody.png)

Note, that you can drag the pink points around to adjust the size and
position of the physics body.

Now we have a CCSprite with the correct image and physics body set up.
Finally, you need to set the custom class property of this interface
file. Once again, this custom class property links an Objective-C class
to this interface file (more on this later). We want the penguin to be
linked to an Objective-C class called "Penguin" so go the right pane,
open the second tab, and set the custom class property:

![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/Spritebuilder_CustomClass.png)

That's it for now - our penguin is ready! You will learn how to
initialize this custom CCSprite subclass very soon!

Seals
=====

You should be able to create the Seal.ccb file on your own now. Repeat
all the steps you did for the penguins:

-   Create a new Interface File (CCSprite as root node)
-   Select the correct sprite image (seal.png)
-   Setup a physics body (a circle as shape will do again)
-   Setup the custom class

Create the classes in Xcode
===========================

It is worth repeating that the "custom class" property of a .ccb file
creates a link between the .ccb file in your SpriteBuilder project and
an Objective-C class in your Xcode project.

For our example this means that whenever someone loads the *Seal.ccb* or
*Penguin.ccb* file, Spritebuilder will initialize the custom classes we
have linked (Seal and Penguin).

To make this work, we need to create the Seal and Penguin class in
Xcode. Open the PeevedPenguins.xcodeproj. Create two new classes (Top
bar: File\>New\>File\>Objective-C Class) called "Penguin" and "Seal" and
make them subclasses of CCSprite because the root nodes of Penguin.ccb
and Seal.ccb are CCSprites as well:

![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/Spritebuilder_Penguin_Xcode.png)

Please create the new file in the *Source* Folder of your project to
keep a consistent structure:

![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/Spritebuilder_Classes_Folder_Location.png)

**Don't forget to repeat this step for the Seal class.**

Testing that everything worked out
==================================

Since we have made a bunch of changes it would be nice to test if
everything worked out as expected before we move on. Most importantly we
want to check if our code connections are working. Publish your
SpriteBuilder project. Next got to Xcode, open "Penguin.m" and add these
lines between @implementation and @end:

    - (id)init {
        self = [super init];
        
        if (self) {
            CCLOG(@"Penguin created");
        }
        
        return self;
    }

Now, everytime the Penguin.ccb file is loaded, "Penguin created" should
appear in the console. Let's do the same for the Seal.m file. You
remembered to create it, right? If not, go ahead and create a new Seal
class in XCode like you did for Penguin. Open it and add these lines
between @implementation and @end:

    - (id)init {
        self = [super init];
        
        if (self) {
            CCLOG(@"Seal created");
        }
        
        return self;
    }

So far, so good. Now for testing purposes we should try to load both
files (penguin.ccb and seal.ccb) from our source code manually to invoke
our custom init methods.

Open the file "AppDelegate.m". Add two lines to the bottom of this
method:

    - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
    {
        [...]
        
        [self setupCocos2dWithOptions:cocos2dSetup];
        
        [CCBReader load:@"Penguin"];
        [CCBReader load:@"Seal"];
        
        return YES;
    }

Now, when our app starts, the penguin.ccb and seal.ccb files should be
loaded immediately, causing a Seal and Penguin object to be initialized.
Two log messages should appear in your console log. Run the app and
check the console for the correct output:

![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/Spritebuilder_CodeConnectionTest.png)

*Note: if the console does not appear, check if you have set the two
options highlighted in the screenshot correctly*

If everything worked out, you should see a "Penguin created" and "Seal
created" message in your console log. **Congratulations!**.

If you don't see the message, please go back and check if you have
performed every step on this page.

Cleaning up & moving on
=======================

You have learned a lot in this chapter. You've set up your first code
connection and now have a basic understanding of how and where the use
of SpriteBuilder and Xcode will overlap in your projects.

Now before we move on to the next chapter, let's clean up our test
methods. **Remove the init methods we have added to Seal.m and
Penguin.m**. Next, **remove the two lines we have added to the
AppDelegate**.
