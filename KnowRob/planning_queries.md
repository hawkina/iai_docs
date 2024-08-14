# KnowRob 2.0 Query Documentation

This file contains the queries which are currently used and have been tested within the scope of the Suturo Project and the GPSR RoboCup demo of the Suturo-Team of the IAI. This document is under constant development. Please let us know if anything stops working/breaks, or if you need queries which are currently not supported.

For every query there are 4 points:

- What the query should do
- query (as written in the pyCRAM KnowRob interface)
- Variables
- Result
- Usage Examples

Queries which are a combination of multiple ones, are prefixed with and (M) in the title and the title corresponds to the python function name.

The baseline for this documentation is this one: https://suturo.github.io/suturo_knowledge/planning-interface/

This is the used KnowRob version: https://github.com/suturo/knowrob

These are the necessary extensions/additions: https://github.com/SUTURO/suturo_knowledge/

Map/Semantic map: https://github.com/SUTURO/suturo_resources/tree/robocup

Launch file for map + semantic map (TODO: double check) https://github.com/SUTURO/suturo_resources/blob/robocup/suturo_bringup/launch/robocup_bringup.launch

The launch file for KnowRob: https://github.com/SUTURO/suturo_knowledge/blob/robocup/suturo_knowledge/launch/suturo_knowledge.launch

---

## has_type

returns the instance of a class (if available) given the iri from the ontology.

```prolog
?- has_type(Instance, '{type_iri}').

```

Variables:

```python
type_iri = '<http://www.ease-crc.org/ont/SUTURO.owl#LivingRoom>'

```

Result:

```prolog
Instance = '<http://www.ease-crc.org/ont/SUTURO.owl#LivingRoom_ABCDEF>'

```

---

## entry_pose / exit_pose

returns the entry/exit_pose of a room. This pose is usually defined right beyond a door.

```prolog
entry_pose('{room_iri}', PoseStamped).
exit_pose('{room_iri}', PoseStamped).

```

Variables:

```python
room_iri = '<http://www.ease-crc.org/ont/SOMA.owl#Kitchen>'

```

Result:

```prolog

```

Returns the entry/exit room pose.

---

### (M) get_nav_poses_for_furniture_item

Returns navigation poses in front of furniture items which the robot can generally use to approach that item and look at it, in order to perceive objects on it.

```prolog
has_type(Room, '{room_iri}'),
(what_object_transitive({furniture_name}, Obj);
has_robocup_name(Obj, {furniture_name})),
has_type(Obj, {furniture_iri}),
Inst = Obj,
is_inside_of(Inst, Room),
furniture_rel_pose(Inst, 'perceive', Pose).

```

Variables:

```python
(optional) room_iri = '<http://www.ease-crc.org/ont/SOMA.owl#Kitchen>' # if not given, _Arena_ is used as a default
(optional) furniture_iri = '<http://www.ease-crc.org/ont/SOMA.owl#CouchTable>' # if not given, #DesignedFurniture is used as a default
furniture_name = 'couch_table' # corresponds to the name given to the object by the _robocup_name_ property

```

Result:
A perceive-pose in front of the furniture item

```prolog

```

---

### (M) check_existence_of_instance

checks if an instance of the given object exists in the world given the nlp name

```prolog
(what_object_transitive('{nlp_name}', Obj),
instance_of(Inst, Obj));
(has_robocup_name(Obj, '{nlp_name}')).

```

Variables:
Result:

---

### (M) check_existence_of_class

returns the class of a given object. Object given uses the NLP Name.

```prolog
what_object_transitive('{nlp_name}', Class).

```

Variables:
Result:

---

### (M) get_predefined_source_item_location_name

get the predefined source location of an item

```prolog
what_object_transitive('{item_name}', Obj),
predefined_origin_location(Obj, Furniture),
furniture_rel_pose(Furniture, 'perceive', Pose).

```

Variables:
Result:

---

### (M) get_predefined_source_item_location_iri

get the predefined source location of an item given the class iri

```prolog
what_object_transitive(Name, {item_iri}),
predefined_origin_location({item_iri}, Furniture),
furniture_rel_pose(Furniture, 'perceive', Pose).

```

Variables:
Result:

---

### (M) get_predefined_destination_item_location

returns the destination location of an item given the item_iri

```prolog
predefined_destination_location({items_iri}, Furniture),
furniture_rel_pose(Furniture, 'perceive', Pose).

```

Variables:
Result:

---

# Queries I would like more information to/am unsure on what exactly they do, but kinda have an intuition. Some of them are buggy or might need more testing:
```python
	kb.prolog_client.once("findall(Room, has_type(Room, soma:'Room'), RoomList).")
    kb.prolog_client.once("member(X,[1,2,3]).")
    kb.prolog_client.all_solutions("member(X,[1,2,3]).")
    kb.prolog_client.all_solutions("entry_pose(Rooms, PoseStamped).")
    kb.prolog_client.all_solutions("middle(Rooms, PoseStamped).")
    kb.prolog_client.once("entry_pose('kitchen', PoseStamped).")  # this works!
    kb.prolog_client.once("entry_pose('kitchen', [Frame, Pose, Quaternion]).")  # this is better
    kb.prolog_client.all_solutions("grasp_pose(ObjectType, Pose).")  # returns bowl = above
    kb.prolog_client.all_solutions("has_value(Objname, Property, Value).")
    kb.prolog_client.all_solutions("predefined_origin_location(Class, OriginLocation).")
    kb.prolog_client.all_solutions("is_inside_of(Obj, Room).")
    # iris ändern sich bei jedem launch
    kb.prolog_client.all_solutions(
        "tf:tf_get_pose('<http://www.ease-crc.org/ont/SOMA.owl#Table_WDOVGYLZ>', ['map', Pos, Rot]).")
    kb.prolog_client.all_solutions(
        "tf:tf_get_pose('<http://www.ease-crc.org/ont/SOMA.owl#Table_WDOVGYLZ>', ['map', Pos, Rot]).")
    kb.prolog_client.all_solutions(
        "has_type(Table, '<http://www.ease-crc.org/ont/SOMA.owl#DesignedHandle>'), is_inside_of(Table,Room), "
        "has_type(Room, suturo:'LivingRoom'), object_rel_pose(Table, 'perceive', Pose).")
    # check for navigation pose to furniture for e.g. searching
    kb.prolog_client.all_solutions(
        "has_type(Obj, '<http://www.ease-crc.org/ont/SOMA.owl#DesignedFurniture>').")  # returns the instances of obj currently present
    kb.prolog_client.all_solutions(
        "triple(Obj, P, '<http://www.ease-crc.org/ont/SOMA.owl#DesignedFurniture>').")  # returns the class names
    # get all obj of type table and their robocup names
    kb.prolog_client.all_solutions(
        "has_type(Obj, soma:'Table'), triple(Obj, suturo:'hasRobocupName', Result).")
    # all semantic map items
    kb.prolog_client.all_solutions("has_urdf_name(Furniture, UrdfLink).")
    # all robocup item names
    kb.prolog_client.all_solutions("has_robocup_name(Furniture, RobocupName).")
    # gets all shelf layers
    kb.prolog_client.all_solutions("has_type(Obj, suturo:'ShelfLayer').")
    # get predefined location
    kb.prolog_client.all_solutions(f"predefined_origin_location(X, Y).")
    # get predifined destination
    kb.prolog_client.all_solutions(f"predefined_destination_location(X, Y).")
    # get all fruits
    kb.prolog_client.all_solutions(f"subclass_of(X, '<http://www.ease-crc.org/ont/SUTURO.owl#RoboCupFruits>').")
    # get obj of that instance by name
    kb.prolog_client.all_solutions(f"what_object('living room', Obj), instance_of(Inst, Obj).")
    # get all classes of obj name
    kb.prolog_client.all_solutions("what_object_transitive('cup', Obj).")
    # get pose of obj
    kb.prolog_client.all_solutions("what_object_transitive('table', Obj), instance_of(Inst, Obj),"
                                   " is_inside_of(Inst, Room), furniture_rel_pose(Inst, 'perceive', Pose).")
    # get poses based on (hopefully) nlp names
    kb.prolog_client.all_solutions(f"what_object_transitive('table', Obj), instance_of(Inst, Obj),"
                                   f"what_object_transitive('living room', Room), instance_of(RoomInst, Room), "
                                   f"is_inside_of(Inst, RoomInst), furniture_rel_pose(Inst, 'perceive', Pose).")

    # triple(Object, soma:isOntopOf, Furniture) # check if obj is on top of shelf layer

    # drop databases
    # kb.prolog_client.all_solutions("drop_graph(user), tf_mem_clear, mng_drop(roslog, tf).")
    # kb.prolog_client.all_solutions(f"reset_user_data.")
```

