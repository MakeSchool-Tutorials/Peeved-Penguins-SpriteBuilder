---
title: Add Physics using Chipmunk, Part 3
slug: collision-detection
---

Now you'll learn how to define different collision types and react to
them. You'll use that knowledge to remove seals when they are crashed by
penguins or ice blocks!

The Cocos2D collision delegate
==============================

Cocos2D already takes care of moving objects around when they collide.
What you as developer need to do is to add *meaning* to different kinds
of collisions. In our game for example, a meaningful collision is one
between a *seal* and any other object in the game.

In Cocos2D we can do this by implementing a certain delegate. In
Objective-C the concept of delegates is always used, when one object
signs up to get informed on certain events. The one we need to use, to
get informed about collisions in Cocos2D is called
*CCPhysicsCollisionDelegate*. Let's add this protocol to *Gameplay.h*,
so that we can get information on the collisions going on in our game:

    @interface Gameplay : CCNode <CCPhysicsCollisionDelegate>

    @end

Now that we implement the protocol, we can sign up as the collision
delegate of our physics node. Add this line to *didLoadFromCCB*:

    _physicsNode.collisionDelegate = self;

Next, we need to set up a collision type for our seal.

Setting up a collision type
===========================

In Cocos2D each *physicsBody* has a property called *collisionType*.
This collisionType is used to identify different participants in a
collision.

Open *Seal.m* to assign a collision type to our seal objects. We're
going to implement this in the *didLoadFromCCB* method again:

    - (void)didLoadFromCCB {
        self.physicsBody.collisionType = @"seal";
    }

Now we will able to identify when seals participate in a collision.

Implementing a delegate method
==============================

Now we need to implement a delegate method that will inform us when a
collision with a seal occurs.

Chipmunk, the physics engine integrated in Cocos2D, gives us 4 different
delegate methods we can implement (all of them are called in a different
step of the collision/simulation). We don't want to dive too deep into
this at the moment, so we will focus on the one delegate method we want
to use:

    -(void)ccPhysicsCollisionPostSolve:(CCPhysicsCollisionPair *)pair typeA:(CCNode *)nodeA typeB:(CCNode *)nodeB;

We are going to use this method, because it provides us information on
how intense the collision is. We only want to remove seals when they
were hit fairly hard.

Chipmunk implemented a clever system for its delegate methods, where it
uses the name of the collision type to generate the name of the delegate
method.

For our example, where we want to be informed about collisions between
seals and all other objects, the method will look like this:

    -(void)ccPhysicsCollisionPostSolve:(CCPhysicsCollisionPair *)pair seal:(CCNode *)nodeA wildcard:(CCNode *)nodeB

The parameter name "seal" in this method is derived from the
*collisionType* "seal". The second parameter "wildcard" means any
arbitrary object. This means, this delegate method is called when an
object with the *collisionType* "seal" collides with any other object.

Now add this method to *Gameplay.m*:

    -(void)ccPhysicsCollisionPostSolve:(CCPhysicsCollisionPair *)pair seal:(CCNode *)nodeA wildcard:(CCNode *)nodeB
    {
        CCLOG(@"Something collided with a seal!");
    }

Run the game and cause a collision with a seal. You should see the log
message appear in the console.

Remove a seal when it get's hit hard
====================================

Now that we know that the collision handler is working, let's implement
the actual functionality. Based on the *totalKineticEnergy* of the
collision, we will decide if a seal will be removed or not. We're also
going to set up a separate seal removal method, because we will want to
add some more functionality to it. 

First import and additional header that we will need for our collision handler code:

    #import "CCPhysics+ObjectiveChipmunk.h"

Then change the collision handling method to have the following content:

    - (void)ccPhysicsCollisionPostSolve:(CCPhysicsCollisionPair *)pair seal:(CCNode *)nodeA wildcard:(CCNode *)nodeB {
      float energy = [pair totalKineticEnergy];

      // if energy is large enough, remove the seal
      if (energy > 5000.f) {
        [[_physicsNode space] addPostStepBlock:^{
          [self sealRemoved:nodeA];
        } key:nodeA];
      }
    }

What exactly are we doing here? First we retrieve the kinetic energy of the collision between the seal and a second object. If this energy is large enough we decide to remove the seal by using the *sealRemoved:* method. But there's one interesting step in between. We call the *space* method of the *_physicsNode* and then call *addPostStepBlock* and use the seal removal method from within there. Why are we doing that? It can happen that two objects collide with a seal within one frame (e.g. an ice block and the ground) in such a case the collision handler method would be called twice. We need to ensure that such a situation does not cause an issue in our code. When we place the collision handling code within a block, that we perform using the *addPostStepBlock* method, Cocos2D will ensure that that code will only be run once per physics calculation. Cocos2D ensures that using the *key* property. Cocos2D will only run one code block per key and frame. With this approach, if the collision handler is called three times, we only call the seal removal method once. 

Finally add the seal removal method to your code:

    - (void)sealRemoved:(CCNode *)seal {
        [seal removeFromParent];
    }

Great! Now you can implement collision handlers to add functionality to
your game!
