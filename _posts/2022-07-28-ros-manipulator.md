---
title: Robotic Arm on ROS
date: 2022-07-29 00:00:00 +0800
categories: [Robotics, ROS]
tags: [ros]     # TAG names should always be lowercase
author: <author_id>
mermaid: true
pin: true
---

# Introduction

The aim of this project is to build a robot manipulator on ROS (Robot Operating System). A robot manipulator is a basically a set of links and joints that together forms an arm. By controlling the movement of the joints and links, the robotic arm can perform tasks such as picking up objects. I made use of `URDF` (Unified Robot Description Format) in order to represent the various components of the manipulator and simulated the same on Rviz and Gazebo.

In order to go about designing the arm, my work is broadly divided into 2 parts:

1. Exploring `URDF` and `Xacro` to make a simple arm
2. Making a seven degree of freedom robotic arm

## Basic manipulator

This model of the manipulator comprises of 6 links and 5 joints, as follows: