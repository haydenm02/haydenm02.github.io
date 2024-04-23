---
title: Virtual Reality Game
date: 2023-05-04 16:00:00
categories: [Education, University]
tags: [vr, c#, unity, games]
---

![Overview of the game](images/2023-05-04-vr-application/overview.png)

The aim for this project was to create a simple VR game to be played without any additional equipment (i.e. keyboards, game controllers or other input devices). This was quite an intriguing task that required some out-of-the-box thinking.

The main hurdles I could identify were the user interface, the movement and the gameplay interactions. I split these up in order to more easily evaluate their challenges and identify solutions.

# User Interface

As our chosen controller contains no forms of buttons or analog inputs, the main source of interaction would be through look. 
A simple way of doing this is to have the player hold their vision in a certain area for a period of time. This would allow for fairly easy selection as long as we give a wide enough target. I chose big, easily readable buttons and provided a crosshair in the center of vision to allow more precise control. The buttons themselves also have visual feedback to show they are being pressed.

![Buttons](images/2023-05-04-vr-application/buttons.png){: .normal }
![Buttons being pressed](images/2023-05-04-vr-application/buttons_pressed.png){: .right}

The same method of buttons is also available above the player during the game:

![Quit button](images/2023-05-04-vr-application/quit.png)

These are simple yet effective alternatives to button presses or mouse clicks.

# Movement

As I would like the player to move, I had to decide between a couple different options. The two big forms of movement in VR games are teleportation or gradual movement/walking. 
I decided that teleportation would be a more engaging form of travel if properly implemented, as it may require less neck movement and be less prone to motion sickness.

The method I settled upon is a teleportation orb, which utilises the same concept as the user interface to both select and throw the orb. 
The player looks at the interface to select the orb, and holds their gaze in a spot for a second to throw it.

![Teleport button](images/2023-05-04-vr-application/tp.png)
![TP button pressed](images/2023-05-04-vr-application/tp_press.png)
![Throw orb](images/2023-05-04-vr-application/throw_tp.png)

As for rotation, I decided upon a clean and easy interface on the borders of the player's vision.
The further targets would turn faster in order to allow for more precise and snappier control of rotation.

![Arrows](images/2023-05-04-vr-application/arrows.png)

# Gameplay

For the gameplay of this project, I decided upon a simple survival game. Move, dodge and attack to survive and get the highest score. 

![Enemy](images/2023-05-04-vr-application/enemy.png)


The player would attack in a similar method as teleportation, throwing orbs that would damage nearby foes. Each foe defeated would increment the score counter and if any get too close they lower the player's HP until they lose.

![Damage orb](images/2023-05-04-vr-application/throw_dmg.png){: .normal }
![Damaged enemy](images/2023-05-04-vr-application/enemy_dmg.png){: .right }

The score would be tallied up and displayed to the player upon the game ending.

![Game over](images/2023-05-04-vr-application/game_over.png)

Overall, this project was a success in demonstrating some of the possibilities of virtual reality in a simple yet engaging game.





