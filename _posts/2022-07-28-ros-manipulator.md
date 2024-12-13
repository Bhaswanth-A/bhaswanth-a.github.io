---
title: Robotic Arm on ROS
date: 2022-07-29 00:00:00 +0800
categories: [Projects, Robotics]
tags: [ros, urdf]     # TAG names should always be lowercase
subsection: "Personal Projects"
author: <author_id>
mermaid: true
pin: false
image: /assets/images/Thumbnail/arm.png
---

[![Bhaswanth-A/robot-manipulator - GitHub](https://gh-card.dev/repos/Bhaswanth-A/robot-manipulator.svg?fullname=)](https://github.com/Bhaswanth-A/robot-manipulator)

# Introduction

The aim of this project is to build a robot manipulator on ROS (Robot Operating System). A robot manipulator is a basically a set of links and joints that together forms an arm. By controlling the movement of the joints and links, the robotic arm can perform tasks such as picking up objects. I made use of `URDF` (Unified Robot Description Format) in order to represent the various components of the manipulator and simulated the same on Rviz and Gazebo.

In order to go about designing the arm, my work is broadly divided into 2 parts:

1. Exploring `URDF` and `Xacro` to make a simple arm
2. Making a seven degree of freedom robotic arm

## Basic manipulator

This model of the manipulator comprises of 6 links and 5 joints, as follows:

### Code

```xml
<?xml version="1.0"?>

<robot name="arm" xmlns:xacro="http://www.ros.org/wiki/xacro">

<!-- Materials -->

<material name="Black">
  <color rgba="0.0 0.0 0.0 1.0"/>
</material>

<material name="Red">
  <color rgba="1.0 0.0 0.0 1.0"/>
</material>

<material name="White">
  <color rgba="1.0 1.0 1.0 1.0"/>
</material>

<link name="base_link">
    <visual>
        <origin rpy="0 0 0" xyz="0 0 0"/>
        <geometry>
            <box size="1 1 1"/>
        </geometry>
        <material name="Black"/>
    </visual>
</link>

<joint name="base_link_link_01" type="revolute">
    <origin rpy="0 0 0" xyz="0 0 0.5"/>
    <parent link="base_link"/>
    <child link="link_01"/>
    <axis xyz="0 0 1"/>
    <limit effort="300" lower="-3.14" upper="3.14" velocity="0.5"/>
</joint>

<link name="link_01">
    <visual>
        <origin rpy="0 0 0" xyz="0 0 0.2"/>
        <geometry>
            <cylinder radius="0.35" length="0.4"/>
        </geometry>
        <material name="Red"/>
    </visual>
    <collision>
        <origin rpy="0 0 0" xyz="0 0 0.2"/>
        <geometry>
            <cylinder radius="0.35" length="0.4"/>
        </geometry>
    </collision>
</link>

<joint name="link_01_link_02" type="revolute">
    <origin rpy="0 0 0" xyz="0 0 0.4"/>
    <parent link="link_01"/>
    <child link="link_02"/>
    <axis xyz="0 1 0"/>
    <limit effort="300" lower="-0.67" upper="0.67" velocity="0.5"/>
</joint>

<link name="link_02">
    <visual>
        <origin rpy="0 0 0" xyz="0 0 0.4"/>
        <geometry>
            <cylinder radius="0.15" length="0.8"/>
        </geometry>
        <material name="White"/>
    </visual>
</link>

<joint name="link_02_link_03" type="revolute">
    <origin rpy="0 0 0" xyz="0 0 0.8"/>
    <parent link="link_02"/>
    <child link="link_03"/>
    <axis xyz="0 1 0"/>
    <limit effort="300" lower="-1.5" upper="1.5" velocity="0.5"/>
</joint>

<link name="link_03">
    <visual>
        <origin rpy="0 0 0" xyz="0 0 0.4"/>
        <geometry>
            <cylinder radius="0.15" length="0.8"/>
        </geometry>
        <material name="Red"/>
    </visual>
    <collision>
        <origin rpy="0 0 0" xyz="0 0 0.4"/>
        <geometry>
            <cylinder radius="0.15" length="0.8"/>
        </geometry>
    </collision>
</link>

<joint name="link_03_link_04" type="revolute">
    <origin rpy="0 0 0" xyz="0 0 0.8"/>
    <parent link="link_03"/>
    <child link="link_04"/>
    <axis xyz="0 1 0"/>
    <limit effort="300" lower="-1.5" upper="1.5" velocity="0.5"/>
</joint>

<link name="link_04">
    <visual>
        <origin rpy="0 0 0" xyz="0 0 0.4"/>
        <geometry>
            <cylinder radius="0.15" length="0.8"/>
        </geometry>
        <material name="Black"/>
    </visual>
    <collision>
        <origin rpy="0 0 0" xyz="0 0 0.4"/>
        <geometry>
            <cylinder radius="0.15" length="0.8"/>
        </geometry>
    </collision>
</link>

<joint name="link_04_link_05" type="revolute">
    <origin rpy="0 0 0" xyz="0 0 0.8"/>
    <parent link="link_04"/>
    <child link="link_05"/>
    <axis xyz="0 1 0"/>
    <limit effort="300" lower="-1.5" upper="1.5" velocity="0.5"/>
</joint>

<link name="link_05">
    <visual>
        <origin rpy="0 0 0" xyz="0 0 0.4"/>
        <geometry>
            <cylinder radius="0.15" length="0.8"/>
        </geometry>
        <material name="White"/>
    </visual>
    <collision>
        <origin rpy="0 0 0" xyz="0 0 0.4"/>
        <geometry>
            <cylinder radius="0.15" length="0.25"/>
        </geometry>
    </collision>
</link>

</robot>
```

### Simulation

The image below shows the simulation of the robotic arm on Rviz, which also provides a GUI that allows me to control the movements of the joints.

![Untitled](/assets/images/Robotic%20Arm%20on%20ROS%20026bd25ddeaf48b2aa5084384c367161/Untitled.png)

The following image shows the different transformation frames and axes of the links, with the X-axis indicated in red, Y-axis in green and Z-axis in blue. 

![Untitled](/assets/images/Robotic%20Arm%20on%20ROS%20026bd25ddeaf48b2aa5084384c367161/Untitled%201.png)

## Seven DOF Arm

The seven DOF robotic arm is a serial link manipulator having multiple serial links. It is kinematically redundant, which means it has more joints and DOF than required to achieve its goal position and orientation. The advantage of redundant manipulators are, we can have more joint configuration for a particular goal position and orientation. It will improve the flexibility and versatility of the robot movement and can implement effective collision free motion in a robotic workspace.

| Joint number | Joint name | Joint type | Limits (rad) |
| --- | --- | --- | --- |
| 0 | fix_world | FIxed | - - - |
| 1 | shoulder_pan_joint | Revolute | -3.14 to 3.14 |
| 2 | shoulder_pitch_joint | Revolute | 0 to 1.899 |
| 3 | elbow_roll_joint | Revolute | -3.14 to 3.14 |
| 4 | elbow_pitch_joint | Revolute | 0 to 1.5 |
| 5 | wrist_roll_joint | Revolute | -1.57 to 1.57 |
| 6 | wrist_pitch_joint | Revolute | -1.5 to 1.5 |
| 7 | gripper_roll_joint | Revolute | -2.6 to 2.6 |
| 8 | finger_joint1 | Prismatic | 0 to 3cm |
| 9 | finger_joint_2 | Prismatic | 0 to 3cm |

### Code

```xml
<?xml version="1.0"?>

<robot name="seven_dof_arm" xmlns:xacro="http://www.ros.org/wiki/xacro">

<!-- Materials -->

<material name="Black">
  <color rgba="0.0 0.0 0.0 1.0"/>
</material>

<material name="Red">
  <color rgba="1.0 0.0 0.0 1.0"/>
</material>

<material name="White">
  <color rgba="1.0 1.0 1.0 1.0"/>
</material>

<xacro:macro name="inertial_matrix" params="mass">  
    <inertial>
        <mass value="${mass}"/>
        <inertia ixx="1.0" ixy="0.0" ixz="0.0" iyy="0.5" iyz="0.0" izz="1.0"/>
    </inertial>
</xacro:macro>

<link name="world"/>

<joint name="fix_world" type="fixed">
  <parent link="world"/>
  <child link="base_link"/>
  <origin xyz="0 0 0" rpy="0 0 0"/>
</joint>

<link name="base_link">
  <visual>
    <origin xyz="0 0 0" rpy="0 0 0"/>
    <geometry>
      <box size="0.1 0.1 0.1"/>
    </geometry>
    <material name="White"/>
  </visual>
  <collision>
    <origin xyz="0 0 0.05" rpy="0 0 0"/>
    <geometry>
      <box size="0.1 0.1 0.1"/>
    </geometry>
  </collision>
  <xacro:inertial_matrix mass="1"/>
</link>

<joint name="shoulder_pan_joint" type="revolute">
  <origin xyz="0 0 0.05" rpy="0 0 0"/>
  <axis xyz="0 0 1"/>
  <parent link="base_link"/>
  <child link="shoulder_pan_link"/>
  <limit effort="300" velocity="0.5" lower="-3.14" upper="3.14"/>
  <dynamics damping="50" friction="1"/>
</joint>

<link name="shoulder_pan_link">
  <visual>
    <origin xyz="0 0 0.02" rpy="0 0 0"/>
    <geometry>
      <cylinder radius="0.04" length="0.04"/>
    </geometry> 
    <material name="Red"/>
  </visual>
  <collision>
    <origin xyz="0 0 0.02" rpy="0 0 0"/>
    <geometry>
      <cylinder radius="0.04" length="0.04"/>
    </geometry>
  </collision>
  <xacro:inertial_matrix mass="1"/>
</link>

<joint name="shoulder_pitch_joint" type="revolute">
  <parent link="shoulder_pan_link"/>
  <child link="shoulder_pitch_link"/>
  <origin xyz="-0.04 0.0 0.04" rpy="0 0 -${3.14/2}" />
  <axis xyz="1 0 0" />
  <limit effort="300" velocity="0.5" lower="0" upper="1.89994105047" />
  <dynamics damping="50" friction="1"/>
</joint>

<link name="shoulder_pitch_link">
  <visual>
    <origin xyz="-0.002 0 0.07" rpy="0 ${3.14/2} 0"/>
    <geometry>
      <box size="0.14 0.04 0.04"/>
    </geometry>
    <material name="White"/>
  </visual>
  <collision>
    <origin xyz="-0.002 0 0.07" rpy="0 ${3.14/2} 0"/>
    <geometry>
      <box size="0.14 0.04 0.04"/>
    </geometry>
  </collision>
  <xacro:inertial_matrix mass="1"/>
</link>

<joint name="elbow_roll_joint" type="revolute">
  <parent link="shoulder_pitch_link"/>
  <child link="elbow_roll_link"/>
  <origin xyz="0.0 0 0.14" rpy="${3.14} ${3.14/2} 0" />
  <axis xyz="-1 0 0" />
  <limit effort="300" velocity="0.5" lower="-3.14" upper="3.14" />
  <dynamics damping="50" friction="1"/>
</joint>

<link name="elbow_roll_link">
  <visual>
    <origin xyz="-0.03 0 0.0" rpy="0 ${3.14/2} 0"/>
    <geometry>
      <cylinder radius="0.02" length="0.06"/>
    </geometry>
    <material name="Black"/>
  </visual>
  <collision>
    <origin xyz="-0.03 0 0.0" rpy="0 ${3.14/2} 0"/>
    <geometry>
      <cylinder radius="0.02" length="0.06"/>
    </geometry>
  </collision>
  <xacro:inertial_matrix mass="1"/>
</link>

<joint name="elbow_pitch_joint" type="revolute">
  <parent link="elbow_roll_link"/>
  <child link="elbow_pitch_link"/>
  <origin xyz="-0.06 0 0" rpy="0 ${3.14/2} 0" />
  <axis xyz="1 0 0" />
  <limit effort="300" velocity="0.5" lower="0" upper="1.5" />
  <dynamics damping="50" friction="1"/>
</joint>

<link name="elbow_pitch_link">
  <visual>
    <origin xyz="0.0 0 -0.11" rpy="0 ${3.14/2} 0"/>
    <geometry>
      <box size="0.22 0.04 0.04"/>
    </geometry>
    <material name="Red"/>
  </visual>
  <collision>
    <origin xyz="0.0 0 -0.11" rpy="0 ${3.14/2} 0"/>
    <geometry>
      <box size="0.22 0.04 0.04"/>
    </geometry>
  </collision>
  <xacro:inertial_matrix mass="1"/>
</link>

<joint name="wrist_roll_joint" type="revolute">
  <parent link="elbow_pitch_link"/>
  <child link="wrist_roll_link"/>
  <origin xyz="0.0 0.0 -0.22" rpy="0 0 0" />
  <axis xyz="1 0 0" />
  <limit effort="300" velocity="0.5" lower="-1.57" upper="1.57" />
  <dynamics damping="50" friction="1"/>
</joint>

<link name="wrist_roll_link">
  <visual>
    <origin xyz="0.0 0.0 -0.02" rpy="0 ${3.14/2} 0"/>
    <geometry>
      <cylinder radius="0.02" length="0.04"/>
    </geometry>
    <material name="Black"/>
  </visual>
  <collision>
    <origin xyz="-0.02 0 0.00" rpy="0 ${3.14/2} 0"/>
    <geometry>
      <cylinder radius="0.02" length="0.04"/>
    </geometry>
  </collision>
  <xacro:inertial_matrix mass="1"/>
</link>

<joint name="wrist_pitch_joint" type="revolute">
  <parent link="wrist_roll_link"/>
  <child link="wrist_pitch_link"/>
  <origin xyz="0.0 0.0 -0.04" rpy="0 0 0" />
  <axis xyz="1 0 0" />
  <limit effort="300" velocity="0.5" lower="-1.5" upper="1.5" />
  <dynamics damping="50" friction="1"/>
</joint>

<link name="wrist_pitch_link">
  <visual>
    <origin xyz="0.0 0.0 -0.03" rpy="0 ${3.14/2} 0"/>
    <geometry>
      <box size="0.06 0.04 0.04"/>
    </geometry>
    <material name="White"/>
  </visual>
  <collision>
    <origin xyz="0 0 -0.03" rpy="0 ${3.14/2} 0"/>
    <geometry>
      <box size="0.06 0.04 0.04"/>
    </geometry>
  </collision>
  <xacro:inertial_matrix mass="1"/>
</link>

<joint name="gripper_roll_joint" type="revolute">
  <parent link="wrist_pitch_link"/>
  <child link="gripper_roll_link"/>
  <origin xyz="0 0 -0.06" rpy="-${1.5*3.14} ${.5*3.14} 0" />
  <axis xyz="1 0 0" />
  <limit effort="300" velocity="0.5" lower="-2.61799387799" upper="2.6128806087" />
  <dynamics damping="50" friction="1"/>
</joint>

<link name="gripper_roll_link">
  <visual>
    <origin xyz="0 0 0.0" rpy="0 -${3.14/2} 0"/>
    <geometry>
      <cylinder radius="0.04" length="0.02"/>
    </geometry>
    <material name="Red"/>
  </visual>
  <collision>
    <origin xyz="0 0 0.0" rpy="0 ${3.14/2} 0"/>
    <geometry>
      <cylinder radius="0.04" length="0.02"/>
    </geometry>
  </collision>
  <xacro:inertial_matrix mass="1"/>
</link>

<joint name="finger_joint1" type="prismatic">
  <parent link="gripper_roll_link"/>
  <child link="gripper_finger_link1"/>
  <origin xyz="0.0 0 0" />
  <axis xyz="0 1 0" />
    <limit effort="100" lower="0" upper="0.03" velocity="1.0"/>

    <safety_controller k_position="20"
                       k_velocity="20"
                       soft_lower_limit="${-0.15 }"
                       soft_upper_limit="${ 0.0 }"/>

  <dynamics damping="50" friction="1"/>
</joint>

<link name="gripper_finger_link1">
  <visual>
    <origin xyz="0.04 -0.03 0"/>
    <geometry>
      <box size="0.08 0.01 0.01"/>
    </geometry>
    <material name="White"/>
  </visual>
  <xacro:inertial_matrix mass="1"/>
</link>

<joint name="finger_joint2" type="prismatic">
  <parent link="gripper_roll_link"/>
  <child link="gripper_finger_link2"/>
  <origin xyz="0.0 0 0" />
  <axis xyz="0 1 0" />
  <limit effort="1" lower="-0.03" upper="0" velocity="1.0"/>

  <dynamics damping="50" friction="1"/>
</joint> 

<link name="gripper_finger_link2">
  <visual>
    <origin xyz="0.04 0.03 0"/>
    <geometry>
      <box size="0.08 0.01 0.01"/>
    </geometry>
    <material name="White"/>
  </visual>
  <xacro:inertial_matrix mass="1"/>
</link>

<!-- Transmission block -->
<xacro:macro name="transmission_block" params="joint_name">
	<transmission name="tran1">
	  <type>transmission_interface/SimpleTransmission</type>
	  <joint name="${joint_name}">
	    <hardwareInterface>hardware_interface/PositionJointInterface</hardwareInterface>
	  </joint>
	  <actuator name="motor1">
	    <hardwareInterface>hardware_interface/PositionJointInterface</hardwareInterface>
	    <mechanicalReduction>1</mechanicalReduction>
	  </actuator>
	</transmission>
</xacro:macro>

<!-- Transmissions for ROS Control -->
<xacro:transmission_block joint_name="shoulder_pan_joint"/>
<xacro:transmission_block joint_name="shoulder_pitch_joint"/>
<xacro:transmission_block joint_name="elbow_roll_joint"/>
<xacro:transmission_block joint_name="elbow_pitch_joint"/>
<xacro:transmission_block joint_name="wrist_roll_joint"/>
<xacro:transmission_block joint_name="wrist_pitch_joint"/>
<xacro:transmission_block joint_name="gripper_roll_joint"/>
<xacro:transmission_block joint_name="finger_joint1"/>
<xacro:transmission_block joint_name="finger_joint2"/>

<!-- Colors in gazebo -->

<gazebo reference="base_link">
  <material>Gazebo/White</material>
</gazebo>

<gazebo reference="shoulder_pan_link">
  <material>Gazebo/Red</material>
</gazebo>

<gazebo reference="shoulder_pitch_link">
  <material>Gazebo/White</material>
</gazebo>

<gazebo reference="elbow_roll_link">
  <material>Gazebo/Black</material>
</gazebo>

<gazebo reference="elbow_pitch_link">
  <material>Gazebo/Red</material>
</gazebo>

<gazebo reference="wrist_roll_link">
  <material>Gazebo/Black</material>
</gazebo>

<gazebo reference="wrist_pitch_link">
  <material>Gazebo/White</material>
</gazebo>

<gazebo reference="gripper_roll_link">
  <material>Gazebo/Red</material>
</gazebo>

<gazebo reference="gripper_finger_link1">
  <material>Gazebo/White</material>
</gazebo>

<gazebo reference="gripper_finger_link1">
  <material>Gazebo/White</material>
</gazebo>

<!-- ros_control plugin -->
<gazebo>
  <plugin name="gazebo_ros_control" filename="libgazebo_ros_control.so">
    <robotNamespace>/seven_dof_arm</robotNamespace>
  </plugin>
</gazebo>

</robot>
```

### Configuring the joint parameters

In a separate configuration file, we add all the joint state and joint position parameters.

```yaml
seven_dof_arm:

  # Joint State Controllers

  joint_state_controller:
    type: joint_state_controller/JointStateController
    publish_rate: 50 # 50Hz

  # Position Controllers assigned to 7 joints and define PID gains

  joint1_position_controller:
    type: position_controllers/JointPositionController
    joint: shoulder_pan_joint
    pid: { p: 100, i: 0.01, d: 10 }

  joint2_position_controller:
    type: position_controllers/JointPositionController
    joint: shoulder_pitch_joint
    pid: { p: 100, i: 0.01, d: 10 }

  joint3_position_controller:
    type: position_controllers/JointPositionController
    joint: elbow_roll_joint
    pid: { p: 100, i: 0.01, d: 10 }

  joint4_position_controller:
    type: position_controllers/JointPositionController
    joint: elbow_pitch_joint
    pid: { p: 100, i: 0.01, d: 10 }

  joint5_position_controller:
    type: position_controllers/JointPositionController
    joint: wrist_roll_joint
    pid: { p: 100, i: 0.01, d: 10 }

  joint6_position_controller:
    type: position_controllers/JointPositionController
    joint: wrist_pitch_joint
    pid: { p: 100, i: 0.01, d: 10 }

  joint7_position_controller:
    type: position_controllers/JointPositionController
    joint: gripper_roll_joint
    pid: { p: 100, i: 0.01, d: 10 }
```

### Launch file

```html
<launch>

    <!-- these are the arguments you can pass this launch file, for example paused:=true -->
    <arg name="paused" default="false"/>
    <arg name="use_sim_time" default="true"/>
    <arg name="gui" default="true"/>
    <arg name="headless" default="false"/>
    <arg name="debug" default="false"/>

    <!-- We resume the logic in empty_world.launch -->
    <include file="$(find gazebo_ros)/launch/empty_world.launch">
    <arg name="debug" value="$(arg debug)" />
    <arg name="gui" value="$(arg gui)" />
    <arg name="paused" value="$(arg paused)"/>
    <arg name="use_sim_time" value="$(arg use_sim_time)"/>
    <arg name="headless" value="$(arg headless)"/>
    </include>

    <!-- Load the URDF into the ROS Parameter Server -->
    <param name="robot_description" command="$(find xacro)/xacro '$(find robot_manipulator)/urdf/seven_dof_arm.xacro'" />

    <!-- Run a python script to the send a service call to gazebo_ros to spawn a URDF robot -->
    <node name="urdf_spawner" pkg="gazebo_ros" type="spawn_model" respawn="false" output="screen" args="-urdf -model seven_dof_arm -param robot_description"/>

    <!-- Load joint controller configurations from yaml file  -->
    <rosparam file="$(find robot_manipulator)/config/seven_dof_arm_control.yaml" command="load"/>

    <!-- Load controllers -->
    <node name="controller_spawner" pkg="controller_manager" type="spawner" respawn="false" output="screen" ns="/seven_dof_arm" 
            args="joint_state_controller 
                joint1_position_controller
                joint2_position_controller
                joint3_position_controller
                joint4_position_controller
                joint5_position_controller
                joint6_position_controller
                joint7_position_controller"/>

    
    <!-- convert joint states to TF transforms for rviz -->
    <node name="robot_state_publisher" pkg="robot_state_publisher" type="robot_state_publisher" output="screen" respawn="false" >
        <remap from="/joint_states" to="/seven_dof_arm/joint_states"/>
    </node>

</launch>
```

**List of available topics:**

![Untitled](/assets/images/Robotic%20Arm%20on%20ROS%20026bd25ddeaf48b2aa5084384c367161/Untitled%202.png)

### Simulation

Launching the robot: `roslaunch robot_manipulator gazebo_seven_dof_arm.launch`

Controlling the joints using the terminal:

`rostopic pub /seven_dof_arm/joint4_position_controller/command std_msgs/Float64 1.0`

![Untitled](/assets/images/Robotic%20Arm%20on%20ROS%20026bd25ddeaf48b2aa5084384c367161/Untitled%203.png)

`rostopic pub /seven_dof_arm/joint1_position_controller/command std_msgs/Float64 1.57`

![Untitled](/assets/images/Robotic%20Arm%20on%20ROS%20026bd25ddeaf48b2aa5084384c367161/Untitled%204.png)

## ROS Controllers

A ROS Controller mainly consists of a feedback mechanism, usually a PID, that lets us control manipulator joints using feedback from the actuators.

ROS controllers are provided by the `ros_control` package. The `ros_control` packages have the implementation of robot controllers, controller managers, hardware interface, different transmission interface, and control toolboxes.

Some standard ROS controllers:

- `joint_position_controller`: implementation of the joint position controller
- `joint_state_controller`: publishes joint states
- `joint_effort_controller`: implementation of the joint effort (force) controller

Common hardware interfaces in ROS:

- Joint Command Interfaces: sends commands to the hardware
    - `EffortJointInterface`: sends the `effort` command
    - `VelocityJointInterface`: sends the `velocity` command
    - `PositionJointInterface`: sends the `position` command
- Joint State Interfaces: retrieves join states from the actuators encoder

### Writing ROS Controllers
