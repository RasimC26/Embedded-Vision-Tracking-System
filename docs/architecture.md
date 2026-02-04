# Embedded Vision Tracking System – Architecture

This project is a real time embedded system that integrates computer vision, embedded control, and interdevice communication. A Raspberry Pi performs camera processing and coordination, while an Arduino handles servo control. The system is designed with clear separation of concerns to ensure reliability and predictable behavior.

## System Components

### Raspberry Pi (Compute Node)
- Captures video frames from camera
- Performs motion detection / object tracking
- Computes target coordinates in image space
- Sends control commands to Arduino over serial
- Hosts a lightweight web dashboard for monitoring

### Arduino (Control Node)
- Receives target position data from Raspberry Pi
- Maps target positions to servo angles
- Enforces physical constraints (angle limits, speed limits)
- Runs a deterministic control loop independent of vision latency

## Communication Protocol

The Raspberry Pi and Arduino communicate over a serial connection using a simple, human-readable text protocol.

Example message:
X:120 Y:85

Design goals:
- Easy to debug during development
- Minimal parsing overhead on Arduino
- Resilient to malformed or delayed messages

## Control Flow

1. Raspberry Pi captures a video frame
2. Vision algorithm detects motion or target object
3. Target coordinates are calculated
4. Coordinates are sent to Arduino via serial
5. Arduino updates servo positions within safety constraints
6. System state is reflected in the web dashboard

## Design Tradeoffs

- Vision processing and motor control are separated to prevent CPU-heavy image processing from affecting real-time actuation.
- Serial communication is used instead of network protocols to reduce latency and complexity.
- Initial implementation prioritizes robustness and debuggability over tracking accuracy.

## Future Improvements

- PID-based smoothing for servo movement
- Object classification to reduce false positives
- Configurable parameters via the web dashboard
- Support for multiple tracking modes

