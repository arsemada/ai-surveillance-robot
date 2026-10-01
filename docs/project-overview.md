# AI-Based Human-in-the-Loop Surveillance Robot

## 1. Project Overview

The AI-Based Human-in-the-Loop Surveillance Robot is a mobile robotic system designed for surveillance and assisted navigation at Shaggar Institute of Technology (SIT).

The robot uses a camera and computer vision to understand its surrounding environment. It identifies relevant objects and the surface/path in front of it, allowing it to move when the path is considered clear.

When the system detects a situation that requires human attention, such as a person or vehicle in the robot's path, it sends an alert to a remote operator through a web-based dashboard.

The operator can then issue commands such as:

- LEFT
- RIGHT
- STOP
- CONTINUE

After a directional command, the robot checks its new environment again before continuing. This creates a human-in-the-loop navigation system where the AI assists with perception while the human operator remains responsible for important navigation decisions.

---

## 2. Problem Statement

Traditional remotely controlled surveillance robots require an operator to continuously control the robot's movement.

On the other hand, a completely autonomous robot requires reliable environment understanding and navigation, which can be difficult to achieve in real-world environments.

This project aims to combine the two approaches.

The robot should be able to move through a suitable path without requiring continuous manual commands, while allowing a human operator to intervene when the AI detects a situation that requires a decision.

The system therefore focuses on:

- Automated environmental perception
- Assisted movement
- Human intervention when required
- Remote monitoring
- Re-evaluation of the environment after operator commands

---

## 3. Proposed Solution

The proposed system consists of a mobile robot equipped with a camera and a computing device.

The camera continuously captures the environment in front of the robot. A computer-vision service processes the camera frames and identifies relevant environmental information.

The initial computer-vision targets are:

- Human/person detection
- Car/vehicle detection
- Road/asphalt/traversable-surface understanding

Based on the detected environment, the system determines whether the robot can continue moving or whether operator intervention is required.

When intervention is required, the backend sends an alert to the web dashboard.

The operator can then select an appropriate command.

After executing a directional command, the robot uses its camera and AI system to evaluate the new direction before continuing.

---

## 4. Project Objectives

### 4.1 Main Objective

To develop a surveillance robot that combines computer vision, automated movement, and human-in-the-loop remote control.

### 4.2 Specific Objectives

1. Capture the robot's surrounding environment using a camera.
2. Detect people and vehicles using computer vision.
3. Identify road, asphalt, or other suitable traversable surfaces.
4. Allow the robot to move when the path is considered clear.
5. Detect situations that require operator intervention.
6. Notify a remote operator through a web dashboard.
7. Allow the operator to issue movement commands remotely.
8. Re-evaluate the environment after a directional command.
9. Record important robot events and operator commands.
10. Provide a foundation for future autonomous navigation improvements.

---

## 5. Core System Workflow

The basic operating workflow is:

```text
Camera
   |
   v
Computer Vision
   |
   v
Environment Understanding
   |
   v
Is the path clear?
   |
   +----------------------+
   |                      |
  YES                     NO
   |                      |
   v                      v
 MOVE               Operator Alert
                          |
                          v
                    Web Dashboard
                          |
              +-----------+-----------+
              |           |           |
             LEFT       RIGHT       STOP
              |
              +-----------+
                    |
                    v
             Robot executes
               command
                    |
                    v
          Camera checks again
                    |
                    v
          Environment analysis
                    |
             +------+------+
             |             |
           CLEAR        NOT CLEAR
             |             |
             v             v
            MOVE      Alert operator


The robot should not assume that a directional command automatically means the new path is safe.

After changing direction, the environment should be checked again.

## 6. Computer Vision

Computer vision is one of the main components of the system.

The initial system will focus on three categories of environmental understanding.

6.1 Human Detection

The AI should detect people within the camera view.

Example:

Person detected
Confidence: 92%
Position: Front-left

A person detection may trigger an operator alert depending on the robot's current state and location of the detected person.

6.2 Vehicle Detection

The AI should detect relevant vehicles, particularly cars.

Example:

Car detected
Confidence: 88%
Position: Front

Vehicle detection can be used to identify situations where the robot should stop or request operator intervention.

6.3 Road / Asphalt / Traversable Surface

The robot needs to understand whether the area in front of it represents a suitable surface for movement.

This should not necessarily be treated as a simple image classification problem.

The system may use a computer-vision approach such as:

Semantic segmentation
Traversable-area detection
Road-surface detection
Other suitable vision-based navigation methods

The final approach will be selected after evaluating available datasets and models.

7. Human-in-the-Loop Control

The project does not aim to make the robot completely autonomous in the initial version.

Instead, the system follows a human-in-the-loop approach.

The AI is responsible primarily for:

Observing the environment
Detecting relevant objects
Understanding the visible path
Identifying situations that require attention

The human operator is responsible for:

Monitoring alerts
Selecting a direction when required
Stopping the robot when necessary
Allowing the robot to continue

The robot then verifies the result of the operator's command using its camera and AI system.

This creates the following cycle:

AI observes
     ↓
AI detects situation
     ↓
Operator is notified
     ↓
Operator makes decision
     ↓
Robot executes command
     ↓
AI observes again
8. Web Dashboard

The operator will control the robot through a web-based dashboard.

The dashboard should provide:

Live Monitoring
Camera feed
Robot status
Current direction
Current environment information
AI Information

Example:

Surface: ASPHALT
Path: CLEAR

Detections:
- Person: 0
- Car: 0

When an object is detected:

⚠ HUMAN DETECTED

Confidence: 94%
Position: Front-right

Operator action required
Robot Controls

The dashboard will provide controls such as:

        [ LEFT ]

[ STOP ]       [ CONTINUE ]

       [ RIGHT ]

The exact interface may change as the system develops.

Event History

The system should record important events such as:

Human detected
Vehicle detected
Robot stopped
Operator selected LEFT
Operator selected RIGHT
Operator selected CONTINUE
Robot resumed movement
9. System Architecture

The initial software architecture is:

                    +----------------------+
                    |       Camera         |
                    +----------+-----------+
                               |
                               v
                    +----------------------+
                    |   AI / CV Service    |
                    |                      |
                    | Person Detection     |
                    | Vehicle Detection   |
                    | Surface Understanding|
                    +----------+-----------+
                               |
                               v
                    +----------------------+
                    |   Decision Engine    |
                    +----------+-----------+
                               |
                               v
                    +----------------------+
                    |   Backend API        |
                    |   Spring Boot        |
                    +----------+-----------+
                               |
                    +----------+-----------+
                    |                      |
                    v                      v
          +----------------+     +----------------+
          |   PostgreSQL   |     | Web Dashboard  |
          |    Database    |     |    Next.js     |
          +----------------+     +----------------+
                                         |
                                         v
                                Remote Operator

The robot-side hardware and software will communicate with the backend through an appropriate communication mechanism.

Possible communication technologies include:

REST API
WebSocket
MQTT
ROS 2

The final communication approach will be selected during implementation based on the requirements of the robot and network environment.

10. Robot States

The robot can be represented using a state-based control system.

Initial states include:

START
  |
  v
MOVING
  |
  +---- Path clear ------> MOVING
  |
  +---- Person detected -> OPERATOR_REQUIRED
  |
  +---- Vehicle detected -> OPERATOR_REQUIRED
  |
  +---- Unsafe condition -> STOPPED

When the operator is required:

OPERATOR_REQUIRED
       |
       +---- LEFT ------> CHECK_PATH
       |
       +---- RIGHT -----> CHECK_PATH
       |
       +---- CONTINUE --> CHECK_PATH
       |
       +---- STOP ------> STOPPED

After a directional command:

CHECK_PATH
    |
    +---- Clear ------> MOVING
    |
    +---- Not clear -> OPERATOR_REQUIRED

This state-machine approach will help keep the robot's behavior predictable and easier to test.

11. Technology Stack
Artificial Intelligence / Computer Vision
Python
PyTorch
OpenCV
YOLO or another suitable object-detection model
Segmentation or traversable-area model where appropriate

The final models will be selected after dataset and model evaluation.

Backend
Java
Spring Boot
Spring Web
PostgreSQL
WebSocket or another suitable real-time communication mechanism
Frontend
Next.js
React
TypeScript or JavaScript
Dashboard UI components
Robot
Raspberry Pi or suitable onboard computer
Camera
Motor controller
Motors
Wheels
Battery
Robot chassis

The physical hardware will be integrated after the software prototype is working on a computer.

12. Development Strategy

Development will begin with a computer-based prototype before connecting the AI system to the physical robot.

Phase 1 — Project Foundation
Create repository
Prepare project documentation
Establish project structure
Define system architecture
Phase 2 — Dataset and AI Research

Identify and evaluate suitable public datasets for:

People
Cars/vehicles
Road/asphalt/traversable surfaces

Evaluate:

Dataset size
Image quality
Labels
Real-world relevance
Licensing
Training/validation/test splits

Select suitable pretrained models or datasets based on the results.

Phase 3 — Computer Vision Prototype

Use a computer webcam to:

Capture frames
Detect people
Detect vehicles
Analyze the road/traversable area
Display detections
Produce an initial movement decision
Phase 4 — Virtual Robot Control

Before connecting physical motors, simulate robot commands.

Example:

AI detects person
       ↓
OPERATOR_REQUIRED
       ↓
Dashboard
       ↓
Operator selects RIGHT
       ↓
Virtual robot turns RIGHT
       ↓
AI checks new path
       ↓
CLEAR
       ↓
MOVE
Phase 5 — Backend

Implement:

Robot status
Detection events
Operator commands
Robot commands
Event logging
Real-time communication
Phase 6 — Web Dashboard

Implement:

Live monitoring
AI detection information
Robot status
Alerts
Direction controls
Event history
Phase 7 — Raspberry Pi Integration

Connect the software prototype to:

Raspberry Pi
Camera
Motor controller
Physical motors
Phase 8 — Physical Testing

Test the robot in a controlled environment.

Testing will begin with simple situations before gradually introducing more complex scenarios.

Phase 9 — Evaluation

Measure:

Detection accuracy
False detections
Response time
Operator command latency
Navigation success
System reliability
Robot response to detected obstacles
13. Minimum Viable Product (MVP)

The first working version should focus on a small and achievable set of capabilities.

MVP Requirements
Laptop webcam provides the camera input.
AI detects people.
AI detects cars.
AI identifies the relevant road/traversable area.
The system determines whether the current path is clear.
The system displays the robot state.
A simulated robot can receive LEFT, RIGHT, STOP, and CONTINUE commands.
The system re-checks the environment after a directional command.
Events are displayed in the dashboard.

The Raspberry Pi and physical robot will be integrated after the computer-based MVP works reliably.

14. Safety Considerations

The AI system should not be treated as the only safety mechanism.

The physical robot should have a reliable emergency stop or manual override mechanism.

Important principles include:

The operator must be able to stop the robot.
The robot should not blindly continue after an uncertain detection.
The robot should verify the environment after changing direction.
Physical safety mechanisms should operate independently of the AI where possible.
Initial physical tests should be performed in a controlled environment.
15. Future Improvements

Future versions may include:

Improved autonomous navigation
Better obstacle avoidance
Additional object classes
Night-time detection
Low-light camera support
GPS integration
Mapping
SLAM
Automatic patrol routes
Multiple camera support
Improved remote monitoring
Long-term event analytics
More advanced path planning
Additional operator controls

These features are outside the initial MVP and will only be added after the core system is working.

16. Current Project Principle

The initial development principle is:

AI observes, the system assists, the human decides when intervention is required, and the robot verifies its environment before continuing.

The project will begin with a computer-based prototype and progressively move toward physical robot integration.