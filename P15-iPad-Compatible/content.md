---
title: Make it iPad compatible
slug: spritebuilder-ipad-compatible
---

This chapter will show you another powerful feature of SpriteBuilder -
you can easily build games that run on smartphones and tablets. Within
the next minutes you will have an iPad version of *Peeved Penguins*.

Time for some new Assets
========================

As of now we are still using *2x* assets in our game. These are used for
the iPhone retina version. If we want to support the iPad retina without
upscaling and blurring our assets we need to provide them in *4x*
resolution.

[Download the new
assets.](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/PeevedPenguinsIPadAssets.zip)
Once you unpacked the new zip file you should see two folders containing
the high-res art.

Open your SpriteBuilder project and remove the two old asset folders you
added throughout this tutorial:

![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/SpriteBuilder_DeleteOldAssets.png)

Next, add the new assets you just downloaded. Drag the *animation* and
the *PeevedPenguinsAssets* folder to the SpriteBuilder project - one at
a time. When you added both folders make the *PeevedPenguinsAssets* a
smart sprite sheet again:

![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/SpriteBuilder_AddNewiPadAssets.png)

Note that you might have to save / re-open your CCB files for the
"missing texture" errors to disappear.

To avoid that images get scaled up on the iPad we need to let
SpriteBuilder know that we are providing *4x* assets now. Let's update
our project settings:

![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/SpriteBuilderSettings4x.png)

Great, we're already half way through the process of making our game
iPad compatible. Publish your changes and run the game on an *iPad
simulator*. The game should look like this:

![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/SpriteBuilder_iPad.png)

This is already really neat. However, two things are off:

1.  The button seems misplaced. It should be in the top left corner.
2.  The user interaction does not behave as expected. You can only touch
    the catapult in the bottom half of the screen.

Let's go ahead and fix these issues.

Fixing the user interaction
===========================

The problem with the user interaction is caused by the root node of our
scene. The root node is not resizing correctly on the iPad. The size of
the root node is currently still defined in *UI Points*. *UI Points* do
not scale up on larger devices. Let's change the size of the
*Gameplay.ccb* root node in SpriteBuilder:

![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/SpriteBuilder_RootNodeSize.png)

We always want to use the full height of the screen, so we choose a *100
%* height. The width of the Gameplay scene should not exceed the
background image - we choose *960 Points*.

Now the root node will resize correctly when running on larger devices
and the user interaction will work just as expected.

Fixing the button position
==========================

This is an easy one. We need to change the *reference corner* for the
button to be the top left corner and set the anchor point to the top
left, too (0.0, 1.0). Then we use the position to define the distance to
the top left corner. We choose (10,10):

![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/SpriteBuilder_ButtonPosition.png)

Now the button's position will always be defined from the top left
corner of the screen. That will improve the layout on the iPad a lot.
Publish the changes & run the game:

![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/SpriteBuilder_iPad_improved.png)

Well done!
==========

You now have a fully functional iPad game. The user interaction works as
expected and your interface looks great across multiple devices!

This section concludes this tutorial for now. Well done! Stay tuned for
further tutorials and additions to this tutorial in future. If you
enjoyed building this game with SpriteBuilder we would like to ask you
to provide a positive review on the [App
Store](https://itunes.apple.com/us/app/spritebuilder/id784912885?mt=12).
Thank you very much for following through!
