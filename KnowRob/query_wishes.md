# Query wishes and Q & A
This document gives overview of user wishes for queries/information. Once they are implemented and tested, they will get moved to their own respective categories.

## NEEM logging
Queries I'd wish for, for the neem logging. The parameter-Names, except maybe the ActionName, should correspond to the keys used within the ActionDesignators in PyCRAM:
- add_action_designator(ActionName, [list_or_dict_of_key_value_pairs]) OR add_action_designator(ActionName, RobotName, Pose, Obj...) *return* ActionID
- add_object_designator(..)
- we need to differenciate between resolved and unresolved designators. Can this somehow be done on KnowRob side or do we need: add_action_designator(..) and add_resolved_action_designator(...)?
- add_location_designator(...)
- add_action_result(ActionID, Result)
- add_role(...) -> I am very unfamiliar with what Roles within KnowRob should do, so please explain what kind of roles we might need in order to log them. They refer to the role of objects during certain tasks I guess?
  ->>> is automatically added/defined in KnowRob. Might need to be added manually.
- add_knowrob_query -> do we want to log what pyCRAM has asked of KnowRob? Or that a Designator has been resolved with KnowRob or something else? Do we need to explicitally call this then or can KnowRob internally log, that it was queried for information during a certain time scope and what the result of that query was?
  

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
