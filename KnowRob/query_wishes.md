# Query wishes and Q & A
This document gives overview of user wishes for queries/information. Once they are implemented and tested, they will get moved to their own respective categories.

## Predefined Object Location - Action Extension
Currently in predefined locations, we get the furniture object returned, on which/in which the object is located. We would need to make a destinction, that if this furniture object is a fridge or a drawer, the door might need to be opened first in order to access that object. How should we solve this? Should there be a property which signals this to PyCRAM? How was this solved previously?

## Object properties
We would like to be able to ask for object properties which influence grasping. Such as: 
- how thick is the object in the area, at which it should be grasped?
- shape of the object (is it a box, cylinder, ball, weird/other shape)
- weight
- texture: is it a rough/smooth object
- softness of object: is it a solid object or a soft one which would give in when grasped, and therefore might need to be grasped tighter?
Some properties like color etc. can be learned from perception but what about the above listed ones?

## People properties (low priority)
it should be possible to add a Person to Knowledge with Properties: Name, favorite drink, (image?), the location they have been last seen at. (e.g. a pose, maybe if NLP said "they are located close to the couch table", and we have found them, then we can also add this information.)
