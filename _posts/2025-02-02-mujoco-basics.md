---
title: MuJoCo Basics
date: 2025-02-02 00:00:00 +0800
categories: [Blog, Robotics]
tags: [mujoco, manipulation, ik, fk]     # TAG names should always be lowercase
author: <author_id>
mermaid: true
pin: false
math: true
image: /assets/images/Thumbnail/mujoco.jpg
---

# Overview of `mjModel` and `mjData`

- `mjModel`: Holds static information about the simulation model (e.g., geometry, joint limits, sensor definitions). Think of it as a blueprint for the robot/environment.
- `mjData`: Holds dynamic state information for the simulation (e.g., joint positions, velocities, applied forces). It tracks how the simulation evolves over time.

## `mjModel`

| **Function** | **Description** | **Example Usage** |
| --- | --- | --- |
| `mj_name2id` | Converts a name (e.g., of a joint, body, or sensor) to its corresponding ID.	 | `joint_id = mj_name2id(model, mjOBJ_JOINT, "joint1")` |
| `mj_id2name` | Converts an ID to its corresponding name.	 | `joint_name = mj_id2name(model, mjOBJ_JOINT, joint_id)` |
| `mj_getGeomId` | Retrieves the ID of a geometry by name. | `geom_id = mj_getGeomId(model, "geom1")` |
| `mj_getBodyId` | Retrieves the ID of a body by name.	 | `body_id = mj_getBodyId(model, "robot_body")` |
| `mj_getJointId` | Retrieves the ID of a joint by name.	 | `joint_id = mj_getJointId(model, "joint1")` |
| `mj_getSiteId` | Retrieves the ID of a site by name.	 | `site_id = mj_getSiteId(model, "end_effector_site")` |
| `mj_printModel` | Dumps the entire model structure to a file.	 | `mj_printModel(model, "model.txt")` |
| `mj_setGeomMass` | Dynamically changes the mass of a geometry.	 | `mj_setGeomMass(model, geom_id, 1.5)` |
| `mj_setBodyMass` | Dynamically changes the mass of a body.	 | `mj_setBodyMass(model, body_id, 10.0)` |
| `mj_setLengthRange` | Sets the length range of a tendon.	 | `mj_setLengthRange(model, tendon_id, 0.1, 0.5)` |

- Use `mj_name2id` and `mj_id2name` to access objects in your model dynamically, when you don’t hardcode object IDs.
- Functions like `mj_setGeomMass` and `mj_setBodyMass` allow you to tweak physical properties during runtime.

### Attributes

| **Function** | **Description** | **Example Usage** |
| --- | --- | --- |
| `qpos0` | Default joint positions (initial state). | `print(model.qpos0)` |
| `qpos_spring` | Rest positions of joints with springs. | `print(model.qpos_spring)` |
| `jnt_range` | Joint position limits, given as `[min, max]` for each joint. | `joint_limits = model.jnt_range` |
| `jnt_type` | Type of each joint (`hinge`, `slide`, etc.). | `print(model.jnt_type)` |
| `body_pos` | Positions of all bodies in the model's local frame. | `body_positions = model.body_pos` |
| `body_quat` | Orientations of bodies in quaternion format. | `body_orientations = model.body_quat` |
| `geom_pos` | Positions of geometries relative to their parent body. | `geom_positions = model.geom_pos` |
| `geom_size` | Dimensions of geometries (e.g., radius for spheres, size for boxes). | `geom_sizes = model.geom_size` |
| `geom_type` | Type of each geometry (`sphere`, `box`, `capsule`, etc.). | `print(model.geom_type)` |
| `dof_damping` | Damping coefficients for degrees of freedom (DOF). | `damping_values = model.dof_damping` |
| `dof_armature` | Armature values for rotational DOF. | `armature_values = model.dof_armature` |
| `actuator_ctrlrange` | Control range (`[min, max]`) for each actuator. | `ctrl_ranges = model.actuator_ctrlrange` |
| `actuator_forcerange` | Force range (`[min, max]`) for each actuator. | `force_ranges = model.actuator_forcerange` |
| `actuator_trntype` | Type of actuator transmission (`joint`, `tendon`, etc.). | `print(model.actuator_trntype)` |
| `sensor_type` | Type of each sensor (`force`, `torque`, etc.). | `sensor_types = model.sensor_type` |
| `sensor_dim` | Dimension of each sensor output (e.g., 3 for a 3D force sensor). | `sensor_dimensions = model.sensor_dim` |
| `sensor_addr` | Index in the `sensordata` array where each sensor's data starts. | `print(model.sensor_addr)` |

The `mjOBJ_JOINT` is a constant identifier in MuJoCo that represents the object type "joint". 

Some common object types and their constants include:

- `mjOBJ_BODY`: Refers to a body.
- `mjOBJ_JOINT`: Refers to a joint.
- `mjOBJ_GEOM`: Refers to a geometry.
- `mjOBJ_SITE`: Refers to a site.
- `mjOBJ_SENSOR`: Refers to a sensor.
- `mjOBJ_ACTUATOR`: Refers to an actuator.
- `mjOBJ_TENDON`: Refers to a tendon.

## `mjData`

| **Function** | **Description** | **Example Usage** |
| --- | --- | --- |
| `mj_resetData` | Resets the simulation to the initial state. | `mj_resetData(model, data)` |
| `mj_step` | Advances the simulation by one timestep. | `mj_step(model, data)` |
| `mj_forward` | Computes forward dynamics without advancing time. | `mj_forward(model, data)` |
| `mj_inverse` | Computes inverse dynamics (forces needed for desired accelerations). | `mj_inverse(model, data)` |
| `mj_applyFT` | Applies a force/torque to a body at a specific point. | `mj_applyFT(model, data, force, torque, body_id, point)` |
| `mj_getControl` | Retrieves the current control inputs. | `controls = mj_getControl(data)` |
| `mj_setControl` | Sets the control inputs for actuators. | `mj_setControl(data, control_values)` |
| `mj_contactForce` | Returns the forces at a specific contact point. | `force = mj_contactForce(model, data, contact_id)` |
| `mj_time` | Returns the current simulation time. | `current_time = mj_time(data)` |
| `mj_energyPos` | Computes the system's potential energy. | `potential_energy = mj_energyPos(model, data)` |
| `mj_energyVel` | Computes the system's kinetic energy. | `kinetic_energy = mj_energyVel(model, data)` |

- Use `mj_step` to progress the simulation. You can modify `data` (e.g., joint positions or control inputs) before calling `mj_step`.
- `mj_contactForce` is useful for analyzing interaction forces in contact-rich environments.

### Attributes

| **Function** | **Description** | **Example Usage** |
| --- | --- | --- |
| `qpos` | Joint positions, representing the current state of the system. | `print(data.qpos)` |
| `qvel` | Joint velocities. | `print(data.qvel)` |
| `qacc` | Joint accelerations. | `print(data.qacc)` |
| `ctrl` | Control inputs applied to actuators. | `data.ctrl[:] = [0.1, 0.2, 0.3]` |
| `qfrc_applied` | Forces applied directly to joints. | `data.qfrc_applied[joint_id] = 1.0` |
| `xfrc_applied` | External forces/torques applied to bodies. | `data.xfrc_applied[body_id][:3] = [1.0, 0.0, 0.0]` |
| `xpos` | Global positions of bodies in the simulation. | `print(data.xpos[body_id])` |
| `xquat` | Global orientations of bodies in quaternion format. | `print(data.xquat[body_id])` |
| `xmat` | Global orientations of bodies in rotation matrix format. | `print(data.xmat[body_id])` |
| `cvel` | Combined spatial velocity (angular + linear) for all bodies in the local frame. | `spatial_velocity = data.cvel[body_id]` \\`angular_velocity = data.cvel[body_id][:3]`\ `linear_velocity = data.cvel[body_id][3:]` |
| `geom_xpos` | Global positions of geometries. | `print(data.geom_xpos[geom_id])` |
| `geom_xmat` | Global orientations of geometries. | `print(data.geom_xmat[geom_id])` |
| `sensordata` | Sensor data output. | `print(data.sensordata)` |
| `subtree_com` | Center of mass of each kinematic subtree. | `print(data.subtree_com[body_id])` |
| `subtree_mass` | Mass of each kinematic subtree. | `print(data.subtree_mass[body_id])` |
| `cfrc_ext` | Contact forces/torques acting on bodies. | `print(data.cfrc_ext[body_id])` |
| `cfrc_int` | Internal forces/torques acting on bodies. | `print(data.cfrc_int[body_id])` |
| `ten_length` | Current lengths of tendons. | `print(data.ten_length[tendon_id])` |
| `ten_velocity` | Velocities of tendons. | `print(data.ten_velocity[tendon_id])` |

## Other MuJoCo Functions

| **Function** | **Description** | **Example Usage** |
| --- | --- | --- |
| `mj_loadXML` | Loads an XML model into MuJoCo. | `model = mj_loadXML("model.xml")` |
| `mj_makeData` | Creates a new `mjData` object for the loaded model. | `data = mj_makeData(model)` |
| `mj_deleteModel` | Frees memory allocated for `mjModel`. | `mj_deleteModel(model)` |
| `mj_deleteData` | Frees memory allocated for `mjData`. | `mj_deleteData(data)` |
| `mjv_updateScene` | Updates the visual representation of the model. | `mjv_updateScene(model, data, options, scene, camera)` |
| `mjr_render` | Renders the scene to a viewport. | `mjr_render(viewport, scene)` |