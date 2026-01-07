---
title: iOS Development
date: 2023-05-02 16:00:00
categories: [Education, University]
tags: [ios, mobile, swift]
---

In this project, I developed a simple game on iOS using XCode and Swift.




# Overview

The goal of this project was to create a simple, learning-focused game on the iOS platform. To do this, I utilised XCode and the Swift programming language.


As Swift provides many OOP features, I was able to pick it up fairly quickly from my experience with Java and C#.

# Navigation

In order to allow for multiple screens, I utilised a Navigation Controller. This allowed me to have
multiple views, one for the main menu and one for the actual game. I linked these using the
button I placed on the main menu and allowed the user to return using the ‘back’ button. In
order to allow for the gameplay elements, I created a subclass view in order to have fine control
of the contents. For example, I could create sprites and elements directly and control how they
interacted

![elements](images/2023-05-02-ios-development/elements.png)
![screens](images/2023-05-02-ios-development/screens.png)

# Development

In order to develop my application, I used a variety of tools in the SpriteKit library of Swift. This
mainly consisted of Nodes for rendering sprites as well as physics bodies in order to simulate
movement and collisions. I also utilised Labels in order to display both the name of the selected
items and the current score.

![toolbar](images/2023-05-02-ios-development/toolbar.png)

The game consists of a ball and a goal. The player drags their finger across the ball in order to
apply a force to it. This will move the ball around the screen. This is affected by both the ball’s
mass (e.g. bowling ball is heavier than football) as well as the tool (e.g. hammer is less powerful
than sledgehammer)

![goal](images/2023-05-02-ios-development/goal.png)

The goal of the game is to drag the ball in order to have it hit the goal. Each time it hits the score
goes up. The player can use the different tools available in order to get the correct power for the
shot.

# iOS vs Android

When developing my application, there was a learning curve to overcome. This was due to a
brand new programming language with many of its own quirks and minor differences to
languages I have used previously.
The main difference would be the null-handling. Swift uses the symbols ? and ! symbols for
checking for null. These are required when variables are potentially null in order to make sure
the compiler knows how to handle them properly. For example, using the exclamation mark (!)
will give a runtime error if the variable is null whereas question mark (?) will allow it to be null.


In my experience, the XCode editor was more interactive and user-friendly than Android Studio.
This allowed me to get started easier and get a working application as fast as possible.
Compared to Android Studio, there were a lot less complex features to overwhelm me so I could
focus on what was required from me