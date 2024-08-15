# NEEM generation Queries
These are queries which are used to generate NEEMs. The purpose of this document is to provide some overview of how these queries work, and what needs to be logged in order to provide a good NEEM.

This is the docs for the following neem-interface version: https://github.com/code-iai/neem-interface/blob/pycram_neem_update/src/neem-interface.pl


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

***
## Workflow
This snippet describes the order in which the following functions need to be called in order to generate a NEEM
1. mem_clear_memory
2. mem_episode_start
3. add_subaction_with_task (now includes mem_action_begin)
4. belief_perceived_at (maybe move this somewhere else, will see)
5. add_participant_with_role
6. mem_action_end
7. mem_action_set_failed /mem_action_set_succeeded
8. mem_episode_stop
***


```prolog
%TODO: Check how CRAM does it, likely called on launch
mem_clear_memory()
```

```prolog
% this starts everything and describes the root action
% returns: An Action which is the root action of the entire NEEM -> so the high level plan Action
% start time is rostime
mem_episode_start(Action, TaskType, EnvOwl, EnvOwlIndiName, EnvUrdf, AgentOwl, AgentOwlIndiName, AgentUrdf)

mem_episode_start(Action, TaskType, EnvOwl, EnvOwlIndiName, EnvUrdf, AgentOwl, AgentOwlIndiName, AgentUrdf, StartTime)
```

```prolog
% stops the recording and drops the NEEM at given path
mem_episode_stop(NeemPath)

mem_episode_stop(NeemPath, EndTime) 
```

```prolog
% set failed or succeeded after the action was completed and after mem_action_end was called
% TODO add the failure reason using fail roc ont
mem_action_set_failed(Action)

mem_action_set_succeeded(Action)
```

```prolog
% Currently not used
mem_action_add_diagnosis(Situation, Diagnosis)
```

```prolog
% ParentAction: the action one has received from mem_episode_start
% TaskType: soma:'Grasping' (ensure correct ontology reference. Can be Soma, can be dul, ...)
% returns: SubAction
mem_add_subaction_with_task(ParentAction,TaskType,SubAction)
```

```prolog
% call this when the Action has started/is finished
mem_action_begin(Action)

mem_action_end(Action) 
```

```prolog
% Call this after an object has been perceived
% Mesh is mesh path with rospath
% ObjectType: soma/dul/suturo:'ObjectType'
% Positon [1,2,3]
% Rotation [0,0,0,1]
% Return: Object
% TODO add tf frame/link
belief_perceived_at(ObjectType, Mesh, Position, Rotation, Object)

belief_perceived_at(ObjectType, Object)
```


```prolog
% This is very important since with this we can link objects to their respective actions
% add a role which is sublac of DUL:Role (check Protege).
% Most used roles: Item, BeneficiaryRole, Query, Agent, Tool...
mem_add_participant_with_role(Action, Object, RoleType)
```
***
### the below functions are in the file but currently are not needed to be called manually for logging

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
***
## Internal / Not needed to be set for logging
```prolog
mem_tf_set(Object, Pose, Timestamp)
```

```prolog
mem_tf_get(Object, Pose)
```

```prolog
mem_tf_get(Object, Pose, Timestamp)
```
