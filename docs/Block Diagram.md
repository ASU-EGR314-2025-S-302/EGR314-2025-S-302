---
title: Block Diagram, Communication Process, Messaging Structure
tags:
- tag1
- tag2
---


## Block Diagram

![Full Team Blocks drawio](https://github.com/user-attachments/assets/6a1368a0-35ca-4a03-a0ee-dbbe892dc35b)

This block diagram defines the general flow of data throughout the subsystem, where the 12V power supply and UART reception and tramissions flow all throughout the daisy chain providing each subsystem with sufficient power and sufficient data. The system starts with the sensor, as it is the subsystem that holds the 12 volt power supply and sends it throughout the daisy chain. It sends the initial data as well, which transports to the OLED subsystem, then to the actuator subsystem, then back, so that each subsystem has access to messages, even if certain messages will only effect some. The OLED will display the actual pressure and target pressure as well as the victory screen when the game is won. The sensor subsystem will include a pressure sensor which communicates pressure back to the OLED subsystem which also uses LEDs to notify the user whether the game is off, on, or won. The motor driver subsystem has a pushbutton which will function as the game's victory button, which will flash the sensor subsystem's green LED and create a victory screen on the OLED.

## Communication Process Diagram

![Sequence Diagram of Team Communication drawio(1)](https://github.com/user-attachments/assets/46d3d484-d8a0-4124-8b83-c66cdafac202)

This diagram defines further the flow of the game, from the start where the sensor system is turned on, all the way to when the pushbutton activates the victory screen and restarts the game. The looping lines reflect the daisy chain connections, as Luke's messages go to Shane's then go to Jack's and then loop back around (if necessary). The initial commands set up the game while the looped commands represent actual game play.

## Messaging Structure

![image](https://github.com/user-attachments/assets/e47349ef-a0b7-4d5d-867e-13db9aa5c6fc)

![image](https://github.com/user-attachments/assets/27645b71-d654-49da-a35d-4ef0720b03cc)

These are the messages that will be sent across UART and the daisy chain to each individual subsystem, defined by number and their content. There are multiple types of messages, including the variable pressure sensor value which is designated to 16 bits, then static values indicating a singular state which are only designated to 8 bits. These are the messages that will be sent in the process represented by the communication diagram above.
