# Project README

## Introduction

This project is an MQTT-based system that uses the MQTT protocol to control both automatic and manual operations of a machine with various components such as nozzles and extension motors. This documentation provides information on how the payload should be structured and the respective topics for each operation.

## Dependencies

- MQTT Broker (like Mosquitto, EMQX, HiveMQ, etc)
- MQTT Client Library (in your preferred language, Paho MQTT is a good choice for most languages)

## MQTT Topics

### 1. Automatic Control

Automatic control payload and topics are designed to automate the machine with a given set of steps.

#### Payload:

- `IAutomaticControl` Interface:
    ```javascript
    interface IAutomaticControl {
        name: string;
        cage_length: number;
        tiers: number;
        drive_mode: "FORWARD" | "BACKWARD"
        steps: IStep[]
    }
    ```
- `IStep` Interface:
    ```javascript
    interface IStep {
        step_number: number,
        active: boolean,
        notification_enabled: boolean,
        tagging:boolean,

        nozzle_height: number,
        
        running_speed: "LOW" | "MEDIUM" | "HIGH"

        // Left/right nozzle rotation (360° continuous rotation or a fixed angle 0°~359°)
        // if value =360 means it is continuous rotation
        left_nozzle_extension: number, // [0-360]
        right_nozzle_extension: number,// [0-360]

        left_nozzle_angle:number,  // [0-180]
        right_nozzle_angle:number, // [0-180]

        left_pump:boolean,
        right_pump:boolean,

        substep?:IStep
    }
    ```

#### Topic:

- `automatic/push`

  The payload for this topic should be `IAutomaticControl`.

---

### 2. Manual Control

Manual control payloads and topics are designed for manual control of the machine.

#### 2.1 Extension Motors

- Payload (`ExtensionMotors` Interface):
    ```javascript
    interface ExtensionMotors {
        value : "DECREASE" | "INCREASE" | "NORMAL_POSITION" | "HOME_POSITION"
    }
    ```
- Topics:
  - `/manual/extension_motors/left`

    The payload for this topic should be `ExtensionMotors`.

  - `/manual/extension_motors/right`

    The payload for this topic should be `ExtensionMotors`.

---

#### 2.2 Nozzle Angle

- Payload (`NozzleAngle` Interface):
    ```javascript
    interface NozzleAngle {
        value : "DECREASE" | "INCREASE" | "NORMAL_POSITION" | "HOME_POSITION"
    }
    ```
- Topics:
  - `/manual/nozzle_angle/left`

    The payload for this topic should be `NozzleAngle`.

  - `/manual/nozzle_angle/right`

    The payload for this topic should be `NozzleAngle`.

---

#### 2.3 Nozzle Rotation

- Payload (`NozzleRotation` Interface):
    ```javascript
    interface NozzleRotation {
        value : "DECREASE" | "INCREASE" | "NORMAL_POSITION" | "HOME_POSITION"  | "CONTINUES_ROTATION"
    }
    ```
- Topics:
  - `/manual/nozzle_rotation/left`

    The payload for this topic should be `NozzleRotation`.

  - `/manual/nozzle_rotation/right`

    The payload for this topic should be `NozzleRotation`.

---
#### 2.4 Nozzle UPP DOWN 
#### 2.5 Running motors 
#### 2.6 Steering Motors
#### 2.7 Hose Reel motors
#### 2.8 Hose Reel Pomp / Valve

## 3. Robot Settings

## 4. Logs


## Setting Up MQTT

To setup MQTT, an MQTT broker should be installed and running. This broker will handle the messages published to it and will deliver them to the respective subscribers.

A MQTT client library is used in the application to establish a connection with the MQTT broker and to publish/subscribe to the topics.

## Implementing the Interfaces

The interfaces provided are meant to be used as data structures for the payloads that will be sent over MQTT. The programming language of your application may not have an interface concept (like Python or C). In such cases, these interfaces are meant to represent a dictionary (Python) or a struct (C) or the equivalent data structure in your language.

Remember, MQTT basically sends and receives byte streams, so the payload needs to be serialized (before sending) and deserialized (after receiving) to/from the format specified in these interfaces.

## Conclusion

This project provides a way to control a machine using both automatic and manual controls via MQTT. The details of the payload structure and the respective topics for each operation are clearly defined in this documentation. This provides a comprehensive guide to setting up and controlling the machine using an MQTT protocol.
