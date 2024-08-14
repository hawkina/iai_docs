# NEEM generation Queries
These are queries which are used to generate NEEMs. The purpose of this document is to provide some overview of how these queries work, and what needs to be logged in order to provide a good NEEM.

## Pre-Conditions for logging
The following parameters have to be set in order to enable logging:
```python
neem_interface = neem_interface_python.neem_interface.NEEMInterface()
task_type = "BreakfastDemo"
env_owl = "package://iai_apartment/owl/iai-apartment.owl"
env_owl_ind_name = "http://knowrob.org/kb/iai-apartment.owl#apartment_root" # ind = individual
env_urdf = "package://iai_apartment/urdf/apartment.urdf"
env_urdf_prefix = "iai_apartment/"
agent_owl = "package://knowrob/owl/robots/PR2.owl"
agent_owl_ind_name = "http://knowrob.org/kb/PR2.owl#PR2_0"
agent_urdf = "package://knowrob/urdf/pr2.urdf"
neem_output_path = "/home/ahawkin/ros_ws/neems_library/BreakfastDemo"
start_time = None
```
ToDo: these parameters might need some explanation. E.g. can the *task_type* be set arbitrary or does it correspond to a SOMA-Task?
How does one find out the *env_owl_ind_name* and what does it correspond to? Is it the root link? Same for the Agent. 


```prolog
mem_clear_memory()
```

```prolog
mem_episode_start(Action, TaskType, EnvOwl, EnvOwlIndiName, EnvUrdf, AgentOwl, AgentOwlIndiName, AgentUrdf)
```

```prolog
mem_episode_start(Action, TaskType, EnvOwl, EnvOwlIndiName, EnvUrdf, AgentOwl, AgentOwlIndiName, AgentUrdf, StartTime)
```

```prolog
mem_episode_stop(NeemPath)
```

```prolog
mem_episode_stop(NeemPath, EndTime) 
```

```prolog
mem_event_set_failed(Action)
```

```prolog
mem_event_set_succeeded(Action)
```

```prolog
mem_event_add_diagnosis(Situation, Diagnosis)
```

```prolog
mem_add_subaction_with_task(ParentAction,SubActionType,TaskType,SubAction)
```

```prolog
belief_perceived_at(ObjectType, Mesh, Rotation, Object)
```

```prolog
belief_perceived_at(ObjectType, Object)
```

```prolog
mem_tf_set(Object, Pose, Timestamp)
```

```prolog
mem_tf_get(Object, Pose)
```

```prolog
mem_tf_get(Object, Pose, Timestamp)
```

```prolog
mem_add_participant_with_role(Action, ObjectId, RoleType)
```

```prolog
add_parameter(Task, ParameterType, RegionType, Parameter)
```

```prolog
add_named_parameter(Task, ParameterType, ParameterName, RegionType, Parameter) 
```

```prolog
add_grasping_parameter(Action,GraspingOrientationType)
```

```prolog
add_comment(Entity,Comment)
```

```prolog
ros_logger_start
```

```prolog
ros_logger_stop
```

