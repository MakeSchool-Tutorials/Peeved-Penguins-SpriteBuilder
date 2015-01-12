---
title: Spritebuilder's User Interface
slug: ui-overview
---

Let's take a tour of SpriteBuilder's User Interface before we finally
delve into creating our game. At first sight, SpriteBuilder's UI feels
quite familiar - it uses many concepts from Xcode/Storyboard.

The interface is divided into 4 main sections:

![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/SBUI.png)

1.  *Resource/Component Browser*: Here you can see the different
    resources and scenes you have created or added to your project. You
    can also select different types of Nodes and drag them into your
    scene.
2.  *Stage*: The stage will preview your current scene. Here you can
    arrange all of the Nodes that belong to a scene.
3.  *Timeline*: The timeline is used to create animations within
    SpriteBuilder. We will look at that in a lot of detail later on.
4.  *Detail View*: Once you select a node in your scene, this detail
    view will display a lot of editable information about that node. You
    can modify positions, content (the text of a label, for example) and
    physics properties.

Let's take a closer look at the most important views.

File View
=========

The first tab in the resource/component browser (labelled as section 1
in the image above) represents the *File View*. It lists all the .ccb
files and resources you have added:

![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/Spritebuilder_FileView.png)

In this view you can add new resources and restructure your project's
folder hierarchy.

Node Library
============

The third tab is the *Node Library*:

![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/Spritebuilder_NodeView.png)

This panel shows you all available node types you can use to construct
your gameplay scenes and menus.

Inspector
=========

The first tab of the Detail View (labelled as section 4 in the above
image) is the Inspector. Once you have selected an object on your stage
you can use this panel to modify many of its properties, like position
and color:

![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/Spritebuilder_Inspector.png)

Code Connections
================

The second tab on the right panel let's you manage code connections for
your selected node. One of the things you can do here is set custom
Objective-C classes for your nodes:

![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/Spritebuilder_CodeConnections.png)

Publishing to Xcode
===================

You already have used this button when testing your first project. Using
the button in the top left corner, you publish your changes in your
Spritebuilder project to your Xcode project. Whenever you changed your
SpriteBuilder project and want to run it, you should hit this button
before building the Xcode project:

![image](https://s3.amazonaws.com/mgwu-misc/Spritebuilder+Tutorial/Spritebuilder_Publish.png)

**Now you're ready to add content to your very first SpriteBuilder
game!**
