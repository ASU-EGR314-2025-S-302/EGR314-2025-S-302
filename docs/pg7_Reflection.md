---
title: Reflection
---

# Lessons Learned

## 1. Start Early

Our team was a bit too nonchalant with the timing when it came to the group project, with our PCB boards coming in close to the final deadline, giving short notice for debugging whatever issues our current boards had and no time for reprints. This caused the team to miss deadlines and let more mistakes get through than ideal. We also bumped into team contribution and communication issues later than we should have because lack of dedication early on, leading us to make sharp and harsh decisions on who would remain in the team. Overall, the project would've been smoother if we had a dedicated plan of action early on.

## 2. Read the Datasheet

Datasheets seem long and confusing to beginners working on their first projects, but they are an integral part to understanding how to choose parts and design a circuit. Without proper knowledge of component specifications, at best you could have a dead-bug board, and at worst have to re-select components and reprint your board entirely. If confronting confusing technical language, either try to understand it through third party sources, or select a component with a more familiar structure and easier to read datasheet.

## 3. Use Many Jumpers/Headers

Electricity can be a scary thing, and unexpected problems may arise with your original board design. If you want to stray away from deadbugging or reprinting your board, jumpers and headers can be a decent way to provide an alternative design if your current design is failing. With jumpers and headers, you can use them as testpoints, or to reroute signal and power lines, or to plug in components from the outside (useful for in-circuit programming and modular expansion)

## 4. Check Part Sizes

Some team members were ignorant of part sizes, and were ordering parts way too small to reasonably solder by hand. Luckily, the professor warned them and they were quick to change their mistake by changing the footprint on the board and the component size. For capacitors/resistors/diodes and the like, it is recommended to go at least 0805 package size for hand soldering.

## 5. Budget Yourself

Our class has a budget limit, and depending on your circuit, it can be quite strict. When purchasing parts, try to fit as much within the budget constraints as can be in case you lose or break parts or your components don't fit your design as intended. Don't work to be ambitious when you're only given a few resources, focus on what works and streamline that to deliver a working product.

## 6. Always Communicate

Our team originally had 4 members, but because of lacking communication and dedication it dropped to 2 in the end. It was particularly hard because our team had to go through multiple design iterations each time a member left, with our system becoming more and more basic. If communication was a bigger priority, then perhaps members would have been more inspired to contribute, and if they still would not contribute, they could have been removed at a less crucial time so the team and the removed member would have time to revise projects.

## 7. Check-off Early

ICCs (In Class Check-offs) while not the main attraction of this course, do contribute a lot of points and gave us a lot of necessary information to create a working system. If neglected, you could likely be missing much of the information taught in class and would have to look to third party resources to learn how to complete the team project, and if your team project is lacking, ICCs could save your grade at the end of the semester, so it is always a good idea to try to get checked off early so you are not caught up during the crunch time of checking off during the due date.

## 8. Talk to Professors/TAs

There is a lot of new information you are going to be learning in this class, and a good amount of it won't be explicitly detailed within the lectures, which will mean you have will have to look for information outside of just lectures. This is why talking to Professors and TAs about your concerns and questions are a good decision. They can provide you information needed to solve any of the problems unique to you that could have been looked over within lecture.

## 9. Heavy Documentation

Engineering is just as much practical application as it is documentation. If a design fails, debug it as well as you can and track what errors arose to cause the failure and why. Even though your project has problems, awareness of the problems and working towards practical solutions shows deep understanding of the work in spite of failure.

## 10. Organize Board Layout

When designing a PCB board, it really helps to neatly organize where all your components are located on the board. This not only helps when drawing taces to limit the risk of crossing paths and the number of vias needed, but also for the actual placement of the hardware. Sometimes you may set up you footprints in a way that looks good but when you actually get the board, your OLED faces the wrong direction from the orientation you wanted, or the motor hangs off the incorrect side. In addition, you have to account for the actual size of components because some may have wider frams than drawn in the foot print, making it harder to solder on other components if that you already soldered the first in.


# Recommmendations for Future Students

### 1. Establish a lasting team communication early on to keep away from later conflicts that could interfere with crucial work.



### 2. Consult as many resources as you can if you cannot solve an issue. If the in class PDFs don't help, try to search outside sources. If outside sources don't help, go to a TA or Professor in order to work hands-on.



### 3. Work just as much on your project early in the semester as you would later in the semester, as it would save you from later stress and possible mistakes.



### 4. Do in-class checkoffs/labs so if your team project suffers you can have your grade fall back on successfully completing the work not directly related to the final project.



### 5. If your team project ends up failing, be thorough in documenting and reporting it to the professors and well as within your github site.



# Version 2.0

In a version 2.0, one thing we could improve would be the ability to send variable types other than ones and zeros as characters. This would be an improvment because it opens up a whole new range of data that we can use to develop a more intricate system that can use things like the input data of a sensor. Curently our communication diagram only has us telling each other to turn systems on or off which works with our current design, but if we wanted to have the speed of the actuator change based on a range of float inputs, we could do that with our current architechture. To add this, all we would need is to modify our send and recieve functions to be able to format the message string in a way that we can still read the payload as its intended variabe type. Dividing up the code would be fairly simple as we would need to adjust the same functions within our own code. In addition, if add some a new device for inputs, we could add the code that handles the input data from it to the HMI system to be sent over to the actuator. To improve debuggability, we found that using LEDs as "Checkpoints" in code is a great way to tell where the code is at while its running as well as telling where it freezes or gets stuck. We can use this to debug our messaging structure by having an LED blink on certain actions such as: receiving a message, sending a message, or running a function based on a payload. This will help us isolate what parts of the code have issue so that we can adjust them as we test. Currently, we don't feel that there are any necessary peripherals needed to improve our current system except for some sort of sensor or microphone if we wanted to return to something similar to our previous designs. For our protocal design, we would modify it to account for a different range of values for inputs and adjust how our actuator moves and what information our OLED will display.
