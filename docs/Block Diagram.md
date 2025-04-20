---
title: Block Diagram, Communication Process, Messaging Structure
tags:
- tag1
- tag2
---


## Block Diagram

![Full Team Blocks drawio](https://github.com/user-attachments/assets/6a1368a0-35ca-4a03-a0ee-dbbe892dc35b)

This block diagram defines the general flow of data throughout the subsystem, where the 12V power supply and UART reception and tramissions flow all throughout the daisy chain providing each subsystem with sufficient power and sufficient data. The system starts with the sensor, as it is the subsystem that holds the 12 volt power supply and sends it throughout the daisy chain. It sends the initial data as well, which transports to the OLED subsystem, then to the actuator subsystem, then back, so that each subsystem has access to messages, even if certain messages will only effect some. The OLED will display the actual pressure and target pressure as well as the victory screen when the game is won. The sensor subsystem will include a pressure sensor which communicates pressure back to the OLED subsystem which also uses LEDs to notify the user whether the game is off, on, or won. The motor driver subsystem has a pushbutton which will function as the game's victory button, which will flash the sensor subsystem's green LED and create a victory screen on the OLED.

## Block Diagram 2

(insert diagram 2)

Due to team complications involving the loss of a member, the block diagram has been simplified to only 2 subsystems. Shane's subsystem will supply 12V power both of the subsystems through pin 1 of the daisy chain. Pin 2 of the daisy chain will be for the RX/TX signals. Shane's output TX signal to Jack's input RX signal and Jack's output TX signal to Shane's input RX signal. The game now starts with the OLED subsystem, where one of the two pushbuttons initialize the game. The screen will display the input pressure and the goal pressure. Input pressure is changed when the user pushes one of the two pushbuttons, one decreasing and one increasing. When the goal pressure is reached, the actuator will receive the data that it has been reached and will move and send the data that the actuator has been moved back to the OLED in order to change target pressure to a different value. When the game has been won, the green LED on Shane's subsystem will flash green and the game will restart.

## Communication Process Diagram

![Sequence Diagram of Team Communication drawio(1)](https://github.com/user-attachments/assets/46d3d484-d8a0-4124-8b83-c66cdafac202)

This diagram defines further the flow of the game, from the start where the sensor system is turned on, all the way to when the pushbutton activates the victory screen and restarts the game. The looping lines reflect the daisy chain connections, as Luke's messages go to Shane's then go to Jack's and then loop back around (if necessary). The initial commands set up the game while the looped commands represent actual game play.

## Messaging Structure

| Byte | Function |
|----|-------|
| AZ | Start |
| YB | Stop  |

| Team Members | Team IDs |
|--------------|----------|
| Jack Francis | J |
| Shane Duttenhefner | S |
| Broadcast | X |

| Message Type | Description |
|--------------|----------|
| 1. Start game | 1 byte for communicating 'on' state to both systems|
| 2. Extend actuator | 1 byte for actuator extension |
| 3. Change target value | 1 byte to change target value |
| 4. Stop game | 1 byte to enable victory screen and reset game |

## Message Type 1

| Byte(s) | Variable Name    | Variable Type | Min Value | Max Value | Example |
|---------|------------------|---------------|-----------|-----------|---------|
| 1       | messagetypeone   | char          | 1         | 1         | 1       |
| 1       | startgame        | char          | 1         | 1         | 1       |

## Message Type 2

| Byte(s) | Variable Name    | Variable Type | Min Value | Max Value | Example |
|---------|------------------|---------------|-----------|-----------|---------|
| 1       | messagetypetwo   | char          | 2         | 2         | 2       |
| 1       | extendactuator   | char          | 0         | 1         | 1       |

## Message Type 3

| Byte(s) | Variable Name    | Variable Type | Min Value | Max Value | Example |
|---------|------------------|---------------|-----------|-----------|---------|
| 1       | messagetypethree | char          | 3         | 3         | 3       |
| 1       | changevalue      | char          | 0         | 1         | 1       |

## Message Type 4

| Byte(s) | Variable Name    | Variable Type | Min Value | Max Value | Example |
|---------|------------------|---------------|-----------|-----------|---------|
| 1       | messagetypefour  | char          | 4         | 4         | 4       |
| 1       | stopgame         | char          | 1         | 1         | 1       |
