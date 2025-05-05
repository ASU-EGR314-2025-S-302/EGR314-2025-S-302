---
title: Block Diagram, Communication Process, Messaging Structure
tags:
- tag1
- tag2
---

## Block Diagram V2

![Block2 drawio](https://github.com/user-attachments/assets/d6e47f94-231a-4d55-a046-69b31c67baba)

Due to team complications involving the loss of a member, the block diagram has been simplified to only 2 subsystems. The original block diagram had 3 subsystems, the third including a pressure sensor that was intended to send pressure values to Jack and Shane's subsystems. Version 1 of the block diagram can be found in the appendix tab. In this updated block diagram, Shane's subsystem will supply 12V power both of the subsystems through pin 1 of the daisy chain. Pin 2 of the daisy chain will be for the RX/TX signals. Shane's output TX signal to Jack's input RX signal and Jack's output TX signal to Shane's input RX signal. The game now starts with the OLED subsystem, where one of the two pushbuttons initialize the game. The screen will display the input pressure and the goal pressure. Input pressure is changed when the user pushes one of the two pushbuttons, one decreasing and one increasing. When the goal pressure is reached, the actuator will receive the data that it has been reached and will move and send the data that the actuator has been moved back to the OLED in order to change target pressure to a different value. When the game has been won, the green LED on Shane's subsystem will flash green and the game will restart.

### Decision Making

The block diagram came together originally by putting together the block diagrams constructed on our individual datasheets, but with some of the extra details reserved just for the individual block diagrams in order to be less cluttered. The block diagram features the necessary functionalities (OLED, HMI, and Actuator) for the system to work and the 8 pin ribbon cable where power and commands are shared.

## Communication Process Diagram V2

![Sequence Diagram of Team Communication Version 2 drawio](https://github.com/user-attachments/assets/04604270-66cf-454e-8033-b0db7ce3deda)

### Functionality

With the recent team changes, we’ve updated our communication diagram to best represent how our board communicates to each other. The user will pick up the game controller that has the OLED display and pushbuttons. Pushing either of the pushbuttons while the OLED is on the start screen will start the game, which will initialize the timer on the actuator, and the OLED will display a randomized target value to the user, along with the current value the user is inputting. When the user pushes the buttons, the input value will go up or down. When the input value hasn't reached the goal value, Shane's subsystem will tell Jack's subsystem to not engage the actuator. When the input value reaches the goal value, Shane's subsystem will send a message to Jack to extend the actuator. After a second of actuator extension, it will stop and a message will be sent to Shane to change the target value, which will display as a new number on Shane's screen. When the actuator has extended far enough, it'll hit a pushbutton on Jack's subsystem, communicating to Shane to display a victory screen and the entire game will reset to the start screen.

# Messaging Structure

## Prefix and Suffix

| Byte | Function |
|----|-------|
| AZ | Start |
| YB | Stop  |

## Message IDs

| Team Members | Team IDs |
|--------------|----------|
| Jack Francis | J |
| Shane Duttenhefner | S |
| Broadcast | X |

## Message Types

| Message Type          | Message ID (char) | Jack Francis (J)                          | Shane Duttenhefner (S)                |
|-----------------------|------------|-------------------------------------------|----------------------------------------------|
| Start game            | 1          | R (LED turns on when message arrives)     | S (pushbutton press triggers send of b'1')   |
| Extend actuator       | 2          | R (actuator extends; verify motion)       | S (interface reaches goal → send b'1')       |
| Change target value   | 3          | S (Delay after extension → send b'1')    | R (HMI displays new target value)            |
| Stop game             | 4          | S (victory button press sends b'1')           | R (HMI shows victory screen and resets game) |

## Message Type 1

This message will be sent from Shane to Jack when a pushbutton is pressed as an indicator that game has started.

| Byte(s) | Variable Name    | Variable Type | Min Value | Max Value | Example |
|---------|------------------|---------------|-----------|-----------|---------|
| 1       | messagetypeone   | char          | 1         | 1         | 1       |
| 2       | startgame        | char          | 0         | 1         | 1       |

## Message Type 2

This message will be sent from Shane to Jack when the interface's input number reaches the goal number in order to extend the actuator.

| Byte(s) | Variable Name    | Variable Type | Min Value | Max Value | Example |
|---------|------------------|---------------|-----------|-----------|---------|
| 1       | messagetypetwo   | char          | 2         | 2         | 2       |
| 2       | extendactuator   | char          | 0         | 1         | 1       |

## Message Type 3

After the actuator is extended, this message will be be sent to Shane from Jack a few seconds after receiving message type 2 so he can change to a new goal number.

| Byte(s) | Variable Name    | Variable Type | Min Value | Max Value | Example |
|---------|------------------|---------------|-----------|-----------|---------|
| 1       | messagetypethree | char          | 3         | 3         | 3       |
| 2       | changevalue      | char          | 0         | 1         | 1       |

## Message Type 4

This message will be sent to Shane after Jack's actuator pushes the victory button which will display a victory screen and reset the game.

| Byte(s) | Variable Name    | Variable Type | Min Value | Max Value | Example |
|---------|------------------|---------------|-----------|-----------|---------|
| 1       | messagetypefour  | char          | 4         | 4         | 4       |
| 2       | stopgame         | char          | 0         | 1         | 1       |

For designing the message structure, we decided to keep the messages simple as to not create unknown communication errors. Most of the communication was turned into 0s and 1s and the more complex math was kept self contained within each subsystem, with the 1s and 0s triggering the process that causes the complex math within a system (for example, instead of the actuator sending a specific pressure value, it will send a 1 to the OLED, which will generate a random number to be the next value). The communication difficulties with previous iterations using more complex variables caused us to move simplify it for more effective communication.

# Top 5 Biggest Software Changes

### 1. Payload Values

Originally, we were going to send over a float that would represent the pressure values being sent over. This became strange, floats were hard to send over serial, even with the struct function. Because of this, we switched over to sending multiple characters but using 0s to pad out the bytes. As our team changed, so did our design and software and it was changed to simple 1 byte 0s and 1s.

### 2. Message Types

There were originally an abundance of message types that would've been sent across the system, many of which did not make it into the final design. There were error messages, status messages, pressure messages, actuator state messages, and so on. As we learned more about board communication, we saw that many of these messages weren't necessary for system functionality. It was then turned into 4 messages to remove the clutter from the message cache to only send data relevant for each system.

### 3. Communication Flow

Our original communication diagram was a lot more complicated given the various types of messages we were sending between our three boards, but as we reduced to two boards and fewer messages, we had to revise our process diagram to a much simpler one. The first iteration had a lot of messages being sent between each board for things like turning on certain systems such as lights and the Display. During revision, however, we found that we could reduce the number of messages as most of those processes could be self contained within our seperate programs.

### 4. SPI to I2C

For the OLED, we had to use SPI in for communication between it and the microcontroller but SPI was rather difficult to work with in this context which became exceptionally more difficult with the use of the PIC microcontroller for the OLED subsystem. Beining unable to get the SPI OLED to turn on, we decided t0 switch over to a different OLED that utilizes I2C communication. The switch over to I2C was fairly easy and kept a reletively similar set up to the original SPI code. In addition, the rginal OLED had a lot more pins involved with programming it, while the new one only had two, so programming the pins in MPLab was a lot easier.

### 5. Getting rid of Pressure sensor
We unfortunately had to the the pressure sensor subsystem that we needed for our previous design. Without the sensor, we no longer need to program our boards to take in ATD inputs. To keep the style of our current game similar to the previous, we still kept the general code structure of our game the same as before, but instead we used simple digital imputs from pushbuttons to control the flow of the game instead of the input readings from the pressure sensor.

