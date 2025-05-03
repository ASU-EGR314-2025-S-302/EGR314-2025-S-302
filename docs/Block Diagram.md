---
title: Block Diagram, Communication Process, Messaging Structure
tags:
- tag1
- tag2
---


## Block Diagram

![Full Team Blocks drawio](https://github.com/user-attachments/assets/6a1368a0-35ca-4a03-a0ee-dbbe892dc35b)

This block diagram defines the general flow of data throughout the subsystem, where the 12V power supply and UART reception and tramissions flow all throughout the daisy chain providing each subsystem with sufficient power and sufficient data. The system starts with the sensor, as it is the subsystem that holds the 12 volt power supply and sends it throughout the daisy chain. It sends the initial data as well, which transports to the OLED subsystem, then to the actuator subsystem, then back, so that each subsystem has access to messages, even if certain messages will only effect some. The OLED will display the actual pressure and target pressure as well as the victory screen when the game is won. The sensor subsystem will include a pressure sensor which communicates pressure back to the OLED subsystem which also uses LEDs to notify the user whether the game is off, on, or won. The motor driver subsystem has a pushbutton which will function as the game's victory button, which will flash the sensor subsystem's green LED and create a victory screen on the OLED.

## Block Diagram V2

![Block2 drawio](https://github.com/user-attachments/assets/d6e47f94-231a-4d55-a046-69b31c67baba)

Due to team complications involving the loss of a member, the block diagram has been simplified to only 2 subsystems. The original block diagram had 3 subsystems, the third including a pressure sensor that was intended to send pressure values to Jack and Shane's subsystems. Version 1 of the block diagram can be found in the appendix tab. In this updated block diagram, Shane's subsystem will supply 12V power both of the subsystems through pin 1 of the daisy chain. Pin 2 of the daisy chain will be for the RX/TX signals. Shane's output TX signal to Jack's input RX signal and Jack's output TX signal to Shane's input RX signal. The game now starts with the OLED subsystem, where one of the two pushbuttons initialize the game. The screen will display the input pressure and the goal pressure. Input pressure is changed when the user pushes one of the two pushbuttons, one decreasing and one increasing. When the goal pressure is reached, the actuator will receive the data that it has been reached and will move and send the data that the actuator has been moved back to the OLED in order to change target pressure to a different value. When the game has been won, the green LED on Shane's subsystem will flash green and the game will restart.

## Communication Process Diagram V2

![Sequence Diagram of Team Communication Version 2 drawio](https://github.com/user-attachments/assets/04604270-66cf-454e-8033-b0db7ce3deda)

With the recent team changes, we’ve updated our communication diagram to best represent how our board communicates to each other. The diagram now starts with the user interacting with push buttons on Shane’s board and then sending a message to Jack to start the game. After that, our boards continue to send messages telling the other to either extend the actuator based on the user’s inputs or update the goal on the display. Once the final push button is hit by the actuator, a final message will be sent from Jack to Shane telling the boards that the game is over and to display the win screen.

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
