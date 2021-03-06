---
title: "GSoC Report 2"
excerpt: "Gamepad Mappings: 2"
header:
  teaser: "/assets/images/teasers/gnome-games.png"

tags:
  - GNOME
  - GSoC
  - GNOME Games
---

We are almost halfway through GSoC. This report is ~2 weeks late but I'll make sure I post another report soon.

## June 15 - July 15

While Adrien reviewed my patches for gamepad mapping system, I worked on the next task - gamepad reassignment.
Adrien suggested loads of changes as he calls it the *"heart of my project"*.

In this post I'll only write about the updates made to gamepad mapping system, and the next post will have gamepad reassignment.

### 1. Patch refinement

I had worked up my patches like a story - I'd make a feature and when I got a design suggestion, I'd change the feature in the next patch.

The result? **25 huge patches**. Adrien helped me get them down to 20 but that's still a troublesome figure.

When I update a patch in the middle of the patch list, I expect bugzilla to reattach that patch in the same place. But what really happens is that the patch goes all the way down the list :/

<figure>
    <a href="https://bugzilla.gnome.org/show_bug.cgi?id=780754"><img src="/assets/images/gnome-games/bugzilla-780754.png"></a>
    <figcaption>So even for minor changes in a patch in the middle I have to update **all** the patches below it.</figcaption>
</figure>

### 2. UI design

It's been a really long journey for the UI design.

<figure>
    <a href="/assets/images/gnome-games/gamepad-mapper-1.png"><img src="/assets/images/gnome-games/gamepad-mapper-1.png"></a>
</figure>

* Fit gamepad mapper inside the preferences window - we don't want three windows simultaneously
* Depopulate the header bar
* Follow the tentative designs at [https://wiki.gnome.org/Design/Apps/Settings](https://wiki.gnome.org/Design/Apps/Settings)
* Create a better gamepad representation (thanks Adrien for the svg!)
* Color the gamepad with style context colors
* Resize gamepad svg on window resize 

<figure>
    <a href="/assets/images/gnome-games/gamepad-mapper-2.png"><img src="/assets/images/gnome-games/gamepad-mapper-2.png"></a>
</figure>

* Allow the user to test the mappings
* Make mapping editor an immersive experience (like selection mode in GNOME Music)
  <figure>
      <a href="/assets/images/gnome-music/selection-mode-1.png"><img src="/assets/images/gnome-music/selection-mode-1.png"></a>
  </figure>

<figure>
    <a href="/assets/images/gnome-games/gamepad-mapper-3-wireframe.png"><img src="/assets/images/gnome-games/gamepad-mapper-3-wireframe.png"></a>
    <figcaption>Adrien has created these wireframes for the new changes. Any suggestions are welcome!</figcaption>
</figure>

### 3. Code design

* Make gamepad mapping system generic  
  Adrien and I planned that gamepad mapping will be useful not only for standard gamepads but other gamepads too.
  I had kept that in mind, but my solution required me to create subclasses of the GamepadView for each Gamepad svg that was needed to be added.

  After a long discussion we came up with Configuration classes for GamepadView. I removed the svg paths and inputs from the gamepad mapping system and instead made it configurable.

* Allow preferences window to show custom header bars.  
  This is required to follow the latest preferences designs. The weird part is that the child which will show the header bar is 3 layers deep in the hierarchy.

  * Preferences Window
    : shows the header bar
    * Preferences Page (Inputs)
      : 1st screen in the wireframe - lists the gamepads
      * Gamepad Configurer
        : 2nd screen in the wireframe - handles gamepad testing and views gamepad mapper
        * Gamepad Mapper
          : 3rd screen in the wireframe - contains the immersive header bar

***

I am still implementing the latest changes so I couldn't add the working gifs/videos in this post. (well! I'm working on GNOME Games so I tend to play on it sometimes too :P)

I'll try to finish gamepad mapping system and gamepad reassignment for GUADEC, so we can see the live demo.

---
