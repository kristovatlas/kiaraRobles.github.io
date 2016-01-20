---
layout: post
comments: true
title: Learning JavaScript from Objective-C (Part 1)
tags: [objective-c]
---
# Learning JavaScript from Objective-C (Part 1)

Before doing my deep dive into Objective-C, I considered about learning JavaScript. I mean why not? JavaScript is sexy. Not only can you do all these amazing things with HTML5 and CSS to build web front-ends, but you can run a backend on Node.js servers, and build universal mobile applications for iOS and android. I didn’t I fully grasp the power of JavaScript until I made a little twitter bot. Still, I resisted but I can't seem to get JavaScript off the mind.

When starting something new that you know is going to take at least several hundred hours to just not completely suck at-it's easy to stop flat at the beginning, fussing about how learning things the right way. I don't know what the best or most efficient way to learn a language is, but I what a fun way is. I'm going to implement some JavaScript array methods in Objective-C.

Because the definition of an array implies some standard functionality similar array methods exist in both JavaScript and Objective-C. Only, the JavaScript methods have much cuter names.

1. **The pop() method** is identical to the Objective-C removeLastObject: method

        - (void)pop
        {
             [self removeLastObject];
        }

In Objective-C, the addObject: method adds only one object at a time. To add multiple objects, you have to call that method on an array in a loop. However, JavaScript allows for adding and subtracting multiple objects to an array with one method call. To implement something similar to this in Objective-C we use a variable method form from C.

2. **The splice() method** adds and or removes items to/from an array

        - (void)splice:(NSUInteger)index remove:(NSUInteger)remove arguments:(id)firstObject,... NS_REQUIRES_NIL_TERMINATION
       {
            if (remove != 0) {
                NSUInteger removeIndex = index;
            
                for (NSUInteger i = 0; i < remove; i++) {
                    [self removeObjectAtIndex:index];
                    removeIndex = removeIndex + 1;
                }
            }
        
            NSUInteger addIndex = index;
            va_list objs;
            va_start(objs, firstObject);
            id nextObj = firstObject;
            while( nextObj ){
                [self insertObject:nextObj atIndex:addIndex];
                addIndex++;
                nextObj = va_arg(objs, id);
            }
            va_end(objs);
        }
            
As in Java and Swift, all array in JavaScript are mutable. So in implementing JavaScript array methods in Objective-C, where there are both mutable and immutable versions of arrays, I built two class categories one on NSArray and another on NSMutableArray. NSMutableArray+JavaScript holds all the methods that manipulate the value of self. While methods in NSArray+JavaScript contain the methods that leave the original array as is, and return either a new array or some other value. The pop() and splice() methods are both examples of mutable array methods. 

Consistent with the cute naming style of JavaScript array methods:

4. **The slice() method** returns the selected elements in an array, as a new array object.
     
        - (NSArray *)slice:(NSUInteger)start stop:(NSUInteger)stop
        {
            NSInteger objectCount = stop - start;
            return [self subarrayWithRange:NSMakeRange(start, objectCount)];
        }

By this point, I have some minor JavaScript envy. Why Objective-C doesn't have a simple reverse() method or this nifty fill() method that replaces all the objects in the array, I don't know. Then again, why JavaScript does have a copyWithin method I don't know either. 

According to the MDN copyWithin:
> The copyWithin() method copies the sequence of array elements within the array to the position starting at a target. The copy is taken from the index positions of the second and third arguments start and end. The end argument is optional and defaults to the length of the array.

Which is some fancy array manipulation method I can't figure out a use case for. Never the less. Here it is implemented below:

5. **The copyWithin() method** 

       - (NSArray *)copyWithin:(NSInteger)target start:(NSInteger)start stop:(NSInteger)stop
       {
            NSInteger objectCount = stop - start;
            NSArray *slicedArray = [self subarrayWithRange:NSMakeRange(start, objectCount)];
            
            NSInteger y = 0;
            for (NSInteger i = target ; i < self.count; i++) {
            
                [self replaceObjectAtIndex:i withObject:slicedArray[y]];
                y = y + 1;
            }
        
            return self;
        }
                    
For the full implementation of two full class categories checkout the GitHub repo [here](https://github.com/kiaraRobles/ENVJavaScript).
