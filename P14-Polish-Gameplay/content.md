---
title: Polish the Gameplay
slug: improve-gameplay
---

Currently a player can only shoot once. The camera will follow the
flying penguin, but won't scroll back to the catapult when the attempt
is completed. We will change that in this chapter. The rules will be the
following: If a penguin leaves the left or right bound of the level, or
if it moves at very slow speed, we will trigger the next attempt to
start.

We will have to check if any of these conditions become *true* on a
regular basis - we can use Cocos2D's update method to do so.

Implementing the update method
==============================

Before we actually implement the update method - add a constant to
*Gameplay.m* that defines the minimum speed which we will use to check
if a penguin is slow enough for the round to end:

    static const float MIN_SPEED = 5.f;

The value '5' works good for our physics setting. If you have changed
the physics properties of your objects you might want to experiment a
little with this value.

Now we can implement the *update* method. This method is automatically
called by Cocos2D every frame.

    - (void)update:(CCTime)delta
    {
        // if speed is below minimum speed, assume this attempt is over
        if (ccpLength(_currentPenguin.physicsBody.velocity) < MIN_SPEED){
            [self nextAttempt];
            return;
        } 
        
        int xMin = _currentPenguin.boundingBox.origin.x;
        
        if (xMin < self.boundingBox.origin.x) {
            [self nextAttempt];
            return;
        }
        
        int xMax = xMin + _currentPenguin.boundingBox.size.width;
        
        if (xMax > (self.boundingBox.origin.x + self.boundingBox.size.width)) {
            [self nextAttempt];
            return;
        }
    }

While this may look a little complicated at first, what actually is
going on is only a tiny bit of math. We check whether the speed is below
our defined limit. Therefore we use the *ccpLength* function that
calculates the square length of our velocity (basically the x- and
y-component of the speed combined). Further we check if the penguin has
exited the level through the left or right boundary. If anything of this
happens, we call the *nextAttempt* method and *return* immediately (to
avoid that *nextAttempt* is called multiple times).

Implementing the *nextAttempt* method
=====================================

The most important thing we need to do in the *nextAttempt* method is
scrolling back to the catapult. However, since we already are running an
action to follow the penguin, we need to stop this action before we
start another scrolling action (otherwise Cocos2D would understandably
be confused about these two conflicting instructions).

Cocos2D provides a method called *stopAction:* that can be called on any
CCNode. However we need a reference (a variable) for the action we want
to stop. So as a first step create a new member variable:

    @implementation Gameplay {
        ...
        
        CCAction *_followPenguin;
    }

Then modify the code in *releaseCatapult* to assign the scrolling action
to this newly created variable:

        // follow the flying penguin
        _followPenguin = [CCActionFollow actionWithTarget:_currentPenguin worldBoundary:self.boundingBox];
        [_contentNode runAction:_followPenguin];

Now we are ready to implement the *nextAttempt* method! Add this method
to *Gameplay.m*:

    - (void)nextAttempt {
        _currentPenguin = nil;
        [_contentNode stopAction:_followPenguin];
        
        CCActionMoveTo *actionMoveTo = [CCActionMoveTo actionWithDuration:1.f position:ccp(0, 0)];
        [_contentNode runAction:actionMoveTo];
    }

First we reset the reference to the *\_currentPenguin*, because once an
attempt is completed we consider none of the penguins as current one.
Then we stop the scrolling action in the second line. Finally we create
a new action to scroll back to the catapult.

Just one more optimization
==========================

You may already have realized that there is one potential problem with
the current solution. Assuming the player pulls back the catapult very
slow a *next attempt* can be triggered **before** the penguin even
launched.

To avoid this happening we will add a flag to the Penguin that stores if
it has been launched already or not.

Open *Penguin.h* and add this property:

    @property (nonatomic, assign) BOOL launched;

To access this new property from *Gameplay.m* we will need to import the
Penguin header by adding this line to the top of the file:

    #import "Penguin.h"

And change the member variable *\_currentPenguin* to be of type
*Penguin* instead of *CCNode*

    Penguin *_currentPenguin;

Now you will also have to change the line in *touchBegan*, where the
penguin is loaded:

    _currentPenguin = (Penguin*)[CCBReader load:@"Penguin"];

You have to change this, because *CCBReader* only returns *CCNodes*, so
if you know that the "Penguin" file actually contains an object of type
*Penguin* you have to add a cast as shown in the line above.

Now that we have access to the *launched* property of the penguin, we
can add a check to the update method, so that everything is only
executed if the penguin has already launched:

    - (void)update:(CCTime)delta
    {
        if (_currentPenguin.launched) {
        ... // <- all previous content of this method belongs inside of this if-statement
        }
    }

Now all the checks will only be performed after firing the penguin.

As a final step we actually need to set the *launched* flag to *TRUE*,
once a penguin is fired. Therefore add this line to the
*releaseCatapult* method:

    _currentPenguin.launched = TRUE;

With this important optimization our *next attempt* mechanism is
completed for now! You just have improved the game quite a bit. Run your
game and confirm that everything works as expected.

You are ready to move on to the next chapter where you will learn how
you can make this game iPad compatible!
