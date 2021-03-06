---
title: "GSoC Report 1"
excerpt: "Gamepad Mappings: 1"
header:
  teaser: "/assets/images/teasers/gnome-games.png"

tags:
  - GNOME
  - GSoC
  - GNOME Games
---

The first two weeks of GSoC coding period have ended. I started to work early in the bonding period and got many things done.

## May 4 - May 30

I was a bit scared after Adrien [ported the gamepad system to C](https://bugzilla.gnome.org/show_bug.cgi?id=782549). I am a huge fan of Vala and had never done Gtk with C before. I spent a lot of time understanding how C with GObject works. GObject has to support many language bindings and hence the C code is **highly** verbose. One thing for sure is that C makes us more aware of many language features that we often take for granted.

After I found and fixed a few bugs that popped up during the porting, I started working on the gamepad mapping configuration.

### What is it?

The goal is to allow the user to configure what each gamepad input does. These are the main components of the gamepad mapper system:

  * Gamepad View  
    Show the standard gamepad and highlight its individual inputs.

  * Gamepad Mapping Builder  
    Build the SDL string from the user provided inputs.

  * Gamepad Mapper  
    Coordinate the work between UI and backend, and display relevant messages. 

  * Gamepad Preferences Window  
    Show the gamepads[^1].

### What is interesting?

* How to highlight gamepad inputs?

  I could have used Adrien's old demo code to manually draw the gamepad:

  <figure>
      <a href="/assets/images/gnome-games/gamepad-highlight-code.png"><img src="/assets/images/gnome-games/gamepad-highlight-code.png"></a>
  </figure>

  But SVGs provide a more accurate representation of the image, and also allow us to query and highlight their elements, so I started looking for relevant libraries.

  *"Do you want to render non-animated SVGs to a Cairo surface with a minimal, no-nonsense API?"* - Librsvg's [README](https://git.gnome.org/browse/librsvg/tree/README)

  **All Librsvg can do for us is to *render* SVGs! How do I get it to highlight stuff?**

  I still favoured Librsvg because it is a part of GNOME. I found two functions of use in Librsvg's [Vala API](https://valadoc.org/librsvg-2.0/Rsvg.Handle.html), one to render an SVG and one to render an SVG sub. I knew it could be exploited somehow.

  * Try 1: Render the gamepad SVG and render the button's sub again  

    What? How did I even expect it to work?

  * Try 2: Render the gamepad SVG and render the button's sub with Cairo.Operator.CLEAR  

    <figure>
        <a href="/assets/images/gnome-games/gamepad-highlight-try-1.png"><img src="/assets/images/gnome-games/gamepad-highlight-try-1.png"></a>
    </figure>

  * Try 3: Paint the surface with blue, render the gamepad SVG and render the button's sub with Cairo.Operator.CLEAR  

    <figure>
        <a href="/assets/images/gnome-games/gamepad-highlight-try-1.png"><img src="/assets/images/gnome-games/gamepad-highlight-try-1.png"></a>
        <figcaption>"Hello darkness, my old friend"<figcaption>
    </figure>

  I tried a lot of things, but before I and darkness became best friends, I understood that with my negligible Cairo skills, I wouldn't be able to do it. So I went to [Cairo's IRC](https://www.cairographics.org/contact/) and @psychon immediately solved my problem.

  * Try 4: Draw button's sub to another surface and use it as a mask to paint the gamepad SVG's surface  

    <figure>
        <a href="/assets/images/gnome-games/gamepad-highlight-try-2.png"><img src="/assets/images/gnome-games/gamepad-highlight-try-2.png"></a>
    </figure>

* SDL string for gamepad mapping

  Here's an example of an SDL string. My task was to build them for user's input mappings.

  ```03000000790000000600000010010000,DragonRise Inc. Generic USB Joystick,platform:Linux, x:b3,a:b2,b:b1,y:b0,back:b8,start:b9, dpleft:h0.8,dpdown:h0.4,dpright:h0.2,dpup:h0.1, leftshoulder:b4,lefttrigger:b6,rightshoulder:b5,righttrigger:b7, leftstick:b10,rightstick:b11, leftx:a0,lefty:a1,rightx:a3,righty:a4,```

  Games was already using the SDL strings provided in [SDL GameControllerDB](https://github.com/gabomdq/SDL_GameControllerDB/) as defaults. I read how Games parsed those strings, and reversed engineered[^2] how to build them.
  
  Let's take one part of it as an example: `start:b9`, if `button 9` is pressed on this gamepad with this mapping, then it will map to the `start` button of the standard gamepad. So while building the mapping, if I highlight the `start` button on the gamepad, and if `button 2` is pressed then I'll build a string with `start:b2`.

  This button to button mapping example was simple but the dpad mappings was a lot more complicated, it took me a while to do it completely.

## June 1 - June 15

I was fairly busy for the first two weeks of June. I made some major enhancements to the previous work but didn't go full steam. My aim is to finish the gamepad mapping configuration for the first GSoC evalution. 

### What is remaining?

* Save the mappings to user directory
* Allow user to reset mappings to default

I also end up doing many things at once. Whenever I find bugs/enhancements in the nearby code, I start working on it. While working on gamepad mapper, I filed new bugs and dived into code that I don't even need for this project.

I will add the working videos/gifs in the next post. Stay tuned :D

---
[^1]: We'll get back to this in July
[^2]: I **had** to use it somewhere, sounds so cool!
