# Around the grasping problem

## Setup

### The scene
- the actuator : arm/hand/finger -> multi-finger
- the targeted objects : rigid, needed to be manipulated to achieve a given task
- the potential obstacles : constraining the actuator/object movements

### The means
- haptic (actuator torque sensor, hand/finger touch sensor)
- vision (RGB, RGB-D, point-cloud, hyper-spectral?)
- on-board, multiple-views...

## Action!

### Task definition
- pick and place
- pick and use : manipulation

### Stages
- recognize and localize the targeted object in the scene
- synthesis a grasp to handle it with the chosen gripper
- plan the motion to perform the task

## Variables

### Prior's object knowledge
- unknown
- familiar
- known

### Object's features
- vision (RGB | point-cloud)
- haptic (contact-points)
- multi-modal

## Technics

### Find the object
- recognition using Convolutional Neural Network
- for known objects (we have its 3D model), estimate its pose
- for familiar objects (we know its class), use dense feature as segmentation
- for unknown object, build a 3D representation based on sensor data (what is a task in this case?)

### Grasp synthesis
- analytical -> solve a mechanical problem in static
- data-driven -> generate candidate grasps (sampling) and select based on a grasp metric (ranking)
- to synthesis grasp quickly : train a machine learning model using grasp generated using a data-driven

### Motion planning
- closed-loop control
- reinforcement learning!
