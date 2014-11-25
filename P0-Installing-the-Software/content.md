---
title: Create your first Project
slug: first-slug
---


#Another Page

Yeah!

![image](http://1.bp.blogspot.com/-TEbZ7-XeJGE/Uwzt5t65JnI/AAAAAAAAWWk/g9gtyGUNZw8/s1600/spritebuilder.png =250x)


	for (int i=0; i < [self.fallingObjects count]; i++) {
        SBBFallingObject *fallingObject = self.fallingObjects[i];
        
        // check if falling object is below the screen boundary
        if (CGRectGetMaxY(fallingObject.boundingBox) < CGRectGetMinY(self.boundingBox)) {
            // if object is below screen, remove it
            [fallingObject removeFromParent];
            [self.fallingObjects removeObject:fallingObject];
            // play sound effect
            [self.animationManager runAnimationsForSequenceNamed:@"DropSound"];
        } else {
            // else, let the object fall with a constant speed
            fallingObject.position = ccp(fallingObject.position.x, fallingObject.position.y - (kFallingSpeed * delta));
        }
        
        // need to convert to local space of catchcontainer
        CGRect containerWorldBoundingBox = CGRectApplyAffineTransform(_catchContainer.boundingBox, _catchContainer.nodeToParentTransform);
        containerWorldBoundingBox = CGRectApplyAffineTransform(containerWorldBoundingBox, CGAffineTransformMakeScale(0.5, 0.5));
        
        // check if falling object is below catching pot
        if (CGRectGetMinY(fallingObject.boundingBox) < CGRectGetMaxY(containerWorldBoundingBox)) {
            if ((CGRectGetMinX(fallingObject.boundingBox) > CGRectGetMinX(containerWorldBoundingBox)) &&
                (CGRectGetMaxX(fallingObject.boundingBox) < CGRectGetMaxX(containerWorldBoundingBox))) {
                // caught the object
                [fallingObject removeFromParent];
            } else {
                // dropped object
            }
        }
    }
