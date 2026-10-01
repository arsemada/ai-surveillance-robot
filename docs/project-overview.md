# AI-Based Human-in-the-Loop Surveillance Robot

## 1. Project Overview

The AI-Based Human-in-the-Loop Surveillance Robot is a software-driven intelligent robotic system designed to monitor and navigate its environment using computer vision, AI-based object detection, path analysis, and remote human control.

The robot uses a camera to observe its surroundings. The AI system analyzes the camera input to identify relevant objects such as people and vehicles and to assess whether the path ahead is suitable for movement.

When the environment is clear, the system can allow the robot to continue moving. When a person, vehicle, obstacle, or uncertain situation is detected, the system can stop the robot and request human intervention through a web-based dashboard.

The human operator can then issue commands such as LEFT, RIGHT, STOP, or CONTINUE. After a directional command is executed, the system re-checks the environment before allowing the robot to continue.

The project follows a human-in-the-loop approach:

> AI observes, the system assists, the human decides when intervention is required, and the robot verifies its environment before continuing.

---

## 2. Problem Statement

Traditional surveillance robots can provide mobility and camera-based monitoring, but an autonomous mobile platform must also understand its environment and respond appropriately to people, vehicles, obstacles, and changing paths.

A fully autonomous approach can be difficult to implement safely and reliably, particularly in environments where unexpected situations may occur.

This project therefore proposes a human-in-the-loop approach in which AI is responsible for environmental perception and decision support, while a remote human operator can intervene whenever the system detects a situation that requires human judgment.

---

## 3. Proposed Solution

The proposed system combines:

- Computer vision
- AI-based object detection
- Path and traversability analysis
- Decision-making logic
- Human-in-the-loop control
- Web-based remote monitoring
- Robot communication and integration

The camera provides visual information to the AI service. The AI service detects relevant objects and analyzes the environment.

The decision engine then determines whether the robot can continue moving or whether operator intervention is required.

The operator interacts with the robot through a web dashboard.

---

## 4. Project Objectives

### 4.1 General Objective

To develop an AI-assisted surveillance robot software system that can perceive its environment, support navigation decisions, and allow remote human intervention.

### 4.2 Specific Objectives

- Detect people and vehicles using computer vision.
- Analyze the area in front of the robot for traversability.
- Determine whether the robot can continue moving.
- Detect situations that require human intervention.
- Provide real-time robot status and detection information.
- Allow an operator to issue directional and safety commands.
- Re-check the environment after a directional command.
- Develop a software interface for communication with a Raspberry Pi-based robot.
- Test the AI and decision-making system before physical robot integration.

---

## 5. Core System Workflow

The main workflow is:

```text
Camera
   ↓
AI Vision Service
   ↓
Object Detection
   ↓
Path / Traversability Analysis
   ↓
Decision Engine
   ↓
┌─────────────────────────────┐
│                             │
│ Path Clear                  │
│       ↓                     │
│     MOVE                    │
│                             │
│ Person / Vehicle /          │
│ Obstacle / Uncertainty      │
│       ↓                     │
│     STOP                    │
│       ↓                     │
│ OPERATOR REQUIRED           │
│       ↓                     │
│ LEFT / RIGHT / STOP /       │
│ CONTINUE                    │
│       ↓                     │
│ Re-check Environment        │
└─────────────────────────────┘

The system should not rely on a single AI prediction for safe operation. The robot should verify the environment again after an operator command before continuing.

6. Computer Vision

The computer vision subsystem is responsible for understanding the camera input.

6.1 Object Detection

The initial object-detection component will use a pretrained YOLO-based model with COCO-trained weights.

The system will initially focus on relevant classes such as:

Person
Car
Bus
Truck
Motorcycle
Bicycle

The project will not require downloading and training the complete COCO dataset from scratch.

Instead, pretrained object-detection weights will be used for the initial prototype.

6.2 Path and Traversability Analysis

Object detection alone is not sufficient for navigation.

The system will also analyze the area in front of the robot to determine whether the available path is suitable for movement.

The project will initially investigate lightweight computer-vision or segmentation approaches for this task.

The objective is to determine:

Whether a usable path exists.
Whether the path is blocked.
Whether a detected object is within the robot's relevant path.
Whether the system should request human intervention.

The path-analysis component can be improved or replaced as testing identifies limitations.

7. Human-in-the-Loop Control

The project does not require the AI to make every navigation decision autonomously.

Instead, the AI provides environmental information and decision support.

When intervention is required, the system notifies the remote operator.

The operator can issue commands such as:

LEFT
RIGHT
STOP
CONTINUE

The robot then executes the appropriate command through its control interface.

After a directional command, the camera and AI system analyze the new environment before the robot resumes movement.

8. Web Dashboard

A web-based dashboard will provide the remote operator with a centralized interface for monitoring and controlling the robot.

The dashboard is expected to provide:

Live camera feed
Robot status
Detected objects
Detection confidence
Path status
Operator alerts
Directional controls
STOP control
CONTINUE control
Event history

Example:

┌──────────────────────────────────────┐
│          ROBOT SURVEILLANCE          │
├──────────────────────────────────────┤
│                                      │
│             LIVE CAMERA              │
│                                      │
├──────────────────────────────────────┤
│ Status: OPERATOR REQUIRED            │
│                                      │
│ Detected: Person                     │
│ Path: BLOCKED                        │
│                                      │
│      [ LEFT ] [ STOP ] [ RIGHT ]    │
│              [ CONTINUE ]            │
└──────────────────────────────────────┘
9. System Architecture

The system will be divided into software, AI, and robot components.

                    CAMERA
                       │
                       ↓
              ┌────────────────┐
              │   AI SERVICE   │
              │                │
              │ YOLO Detection │
              │ Path Analysis  │
              └───────┬────────┘
                      │
                      ↓
              ┌────────────────┐
              │ Decision Engine│
              └───────┬────────┘
                      │
             ┌────────┴────────┐
             ↓                 ↓
       Spring Boot         Web Dashboard
          Backend              │
             │                 │
             └────────┬────────┘
                      │
                  Robot API
                      │
                      ↓
               Raspberry Pi
                      │
          ┌───────────┼───────────┐
          ↓           ↓           ↓
       Camera      4 DC Motors  Servo
10. Software Responsibilities

The project software will primarily be divided into the following components.

AI Service

Responsible for:

Camera/image processing
Object detection
Path analysis
AI inference
Detection results
Decision Engine

Responsible for:

Interpreting AI results
Determining robot state
Deciding when operator intervention is required
Generating robot commands
Backend

Responsible for:

Robot communication
Command APIs
Robot state
AI result communication
Event management
Real-time communication
Frontend

Responsible for:

Remote monitoring
Live status
Detection visualization
Operator alerts
Robot commands
Robot Integration

The physical robot will be implemented separately using a Raspberry Pi-based platform.

The planned hardware includes:

Raspberry Pi
Camera
4 DC motors
Servo motor

The software system will communicate with the Raspberry Pi through a defined communication interface.

The exact motor-control implementation will be handled as part of the robot/hardware subsystem.

11. Robot States

The system will use a state-based decision model.

Initial states include:

START
  ↓
MOVING
  ↓
CHECKING_ENVIRONMENT
  ↓
┌───────────────────────┐
│                       │
│ CLEAR                 │
│   ↓                   │
│ MOVING                │
│                       │
│ PERSON / VEHICLE /    │
│ OBSTACLE / UNCERTAIN  │
│   ↓                   │
│ OPERATOR_REQUIRED     │
│                       │
└───────────────────────┘
          ↓
    LEFT / RIGHT /
    STOP / CONTINUE
          ↓
    CHECKING_ENVIRONMENT
          ↓
       MOVING
12. Technology Stack
AI / Computer Vision
Python
PyTorch
OpenCV
YOLO-based object detection
COCO-pretrained model weights
Segmentation or other path-analysis techniques
Backend
Java
Spring Boot
REST API
WebSocket or another real-time communication mechanism
PostgreSQL
Frontend
Next.js
React
TypeScript/JavaScript
Robot
Raspberry Pi
Camera
4 DC motors
Servo motor

The final communication protocol between the software system and Raspberry Pi will be determined during the integration phase.

13. Development Strategy

The project will be developed incrementally.

Phase 1 — Project Foundation
Project documentation
Repository structure
Development environment
Phase 2 — AI Vision Prototype
Set up AI service
Load pretrained YOLO model
Test person and vehicle detection
Test camera input
Develop initial path-analysis approach
Phase 3 — Decision Engine
Define robot states
Implement movement decisions
Implement operator intervention logic
Implement simulated robot commands
Phase 4 — Backend
Create Spring Boot backend
Implement robot command APIs
Implement robot status APIs
Connect AI service with backend
Implement real-time communication
Phase 5 — Web Dashboard
Live monitoring
Detection display
Robot status
Operator alerts
Directional controls
Phase 6 — Raspberry Pi Integration
Define communication protocol
Connect software to Raspberry Pi
Send movement commands
Receive robot/camera information
Test communication reliability
Phase 7 — Physical Robot Testing
Integrate with the physical robot
Test movement commands
Test camera input
Test AI detection
Test operator intervention
Test path re-checking
Phase 8 — Evaluation

The final system will be evaluated using measures such as:

Object detection accuracy
False detections
Path-analysis performance
Decision response time
Command latency
Robot navigation success
System reliability
14. Minimum Viable Product (MVP)

The first working version will not require the physical robot.

The MVP will use a laptop/PC camera to demonstrate:

Person detection.
Vehicle detection.
Camera-based environment analysis.
Basic path assessment.
MOVE decision when the path is clear.
STOP/OPERATOR REQUIRED decision when intervention is needed.
Simulated LEFT, RIGHT, STOP, and CONTINUE commands.
Re-checking the environment after a directional command.

After the software MVP is validated, it will be connected to the Raspberry Pi-based robot.

15. Safety Considerations

The AI system is intended to provide perception and decision support.

It should not be treated as the only safety mechanism for the physical robot.

The physical robot should have an independent emergency stop/manual override mechanism.

Testing should initially be performed in controlled environments and at low speed.

16. Future Improvements

Possible future improvements include:

Better path segmentation
SIT-specific dataset collection
Fine-tuning models using local data
Additional obstacle classes
Improved navigation
GPS/location support
Mapping
Autonomous route planning
Night-time detection
Low-light enhancement
Improved real-time communication
Multi-camera support
Event recording
Remote access and authentication
17. Current Project Principle

The project follows the principle:

AI observes, the system assists, the human decides when intervention is required, and the robot verifies its environment before continuing.