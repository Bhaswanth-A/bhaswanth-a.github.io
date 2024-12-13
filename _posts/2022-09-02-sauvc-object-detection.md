---
title: Object Detection
date: 2022-09-01 00:00:00 +0800
categories: [Projects, Robotics]
tags: [dl, cnn, computer vision, python, yolo]     # TAG names should always be lowercase
subsection: "SAUVC"
author: <author_id>
mermaid: true
pin: false
image: /assets/images/ldr.png
---

Object detection is a computer technology related to computer vision and image processing that deals with detecting instances of semantic objects of a certain class in digital images and videos. One popular algorithm for object detection is YOLO (You Only Look Once). YOLO uses a single neural network to predict bounding boxes and class probabilities directly from full images in one evaluation. It's become popular in real-time object detection because of its speed, as well as its ability to detect a large number of object classes.

Using YOLO for object detection in an underwater environment, however, is a challenging task. The lighting conditions, turbidity, and low visibility can affect the performance of the algorithm. In addition, underwater images may have different characteristics than images taken in an above-water environment, such as color distortion, refraction, and reflection. To overcome these challenges, image pre-processing and training the model on a dataset of underwater images is crucial. This dataset should include a variety of underwater scenarios, lighting conditions, and different types of objects.

In the context of the Singapore Autonomous Underwater Vehicle Challenge (SAUVC), YOLO can be used to detect and classify various objects in the underwater environment. This can help to improve the navigation and control of the autonomous underwater vehicle (AUV). For example, YOLO can be used to detect obstacles and map the environment, which can be used to plan a safe and efficient path for the AUV to follow. YOLO can also be used to identify targets of interest, such as underwater structures or objects of a specific class. Furthermore, object detection information can be used to trigger specific actions or behaviors of the AUV, such as obstacle avoidance, and capturing images or samples of a specific object.

In the SAUVC competition, the vehicle is required to navigate in an unknown and unstructured environment. The object detection algorithm can be used for mission planning and localization and can help the vehicle perform more complex tasks, such as search and recovery.