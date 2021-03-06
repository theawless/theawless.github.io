---
title: "GSoC Report 4"
excerpt: "Controller Reassignment"
header:
  teaser: "/assets/images/teasers/gnome-games.png"

tags:
  - GNOME
  - GSoC
  - GNOME Games
---

This report is about Controller Reassignment.

Previously, Games used to order controllers according to how they were plugged in. So. if I want to be the P1 (which I always want), I can simply exchange the controller with my brother. But hey, what if he is sitting 5 feet away from me?

So a feature that allows us to reorder gamepads without unpluggings or exchanges is very very important.

### What is it?

There are three components to the system:

* Controller Popover  
  Show the current controllers to the user and an option to reassign.

  <figure>
      <a href="/assets/images/gnome-games/controller-popover.png"><img src="/assets/images/gnome-games/controller-popover.png"></a>
  </figure>

* Controller Reassigner  
  Reorder the controllers (if a button on Gamepad A is pressed before a button on Gamepad B then Gamepad A will be ordered higher than Gamepad B).

  <figure>
      <a href="/assets/images/gnome-games/controller-reassigner.png"><img src="/assets/images/gnome-games/controller-reassigner.png"></a>
  </figure>

* Controller Set  
  Coordinate the controller ordering between backend and UI.


### What is interesting?

Controller Set was pretty hard to design. It contains the controllers and their respective ports. There's keyboard, mouse input and gamepads. Currently, Games does not have a common interface of inputs for me to use, and I had to use loads of ifs and elses in my code.

Games has Retro Input Manager that manages the ordering of the controllers. Whenever a controller is plugged in or out, Retro Input Manager does the magic to order it in the right place.

When user is reassigning the controllers, a new instance of Controller Set is created in Controller Reassigner and the ports are assigned according to user's button press.
Then the Controller Set in Retro Input Manager is replaced with this new Controller Set.

* Retro Input Manager (for plug ins and outs) -> works on Controller Set 
* Controller Reassigner (for user ordering) -> works on Controller Set

What happens when the user creates an ill defined Controller Set? Say he attached a controller and assigned it a port and then removed the controller. The plug ins and outs were supposed to be handled by Retro Input Manager, but the Controller Set in Retro Input Manager hasn't yet been replaced with the new one created by user!

### What is remaining?

A prototype is ready on my branch [gamepad-reassign](https://git.gnome.org/browse/gnome-games/log/?h=wip/abhinavsingh/gamepad-reassign) and it works well barring the edge cases. But a lot of work needs to be done on the design of this feature (both code wise and looks wise).

---

GSoC period is coming to an end and I will wrap up in the next post.

---
