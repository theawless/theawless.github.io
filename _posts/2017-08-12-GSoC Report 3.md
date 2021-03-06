---
title: "GSoC Report 3"
excerpt: "GUADEC + Gamepad Mappings: 3"
header:
  teaser: "/assets/images/teasers/gnome-games.png"

tags:
  - GNOME
  - GUADEC
  - GSoC
  - GNOME Games
---

I was not able to visit GUADEC this year :'(

**But hey, [suhas2go](https://suhas2go.github.io) got me some goodies, so kudos to him!**

<figure class="half">
    <a href="/assets/images/gnome/guadec-2017-stickers.jpg"><img src="/assets/images/gnome/guadec-2017-stickers.jpg"></a>
    <a href="/assets/images/gnome/guadec-2017-tees.jpg"><img src="/assets/images/gnome/guadec-2017-tees.jpg"></a>
    <figcaption>PS: Suhas I'm not paying you back for the tees</figcaption>
</figure>

Gamepad mappings code got pushed after going through many review iterations. Here's a demo that I would have given, had I came to the conference.

### Demonstration

* Show a list of controllers
  <figure>
      <a href="/assets/images/gnome-games/preferences-page-controller.webm"><iframe width="637" height="435" src="" frameborder="0" src="/assets/images/gnome-games/preferences-page-controller.webm" allowfullscreen></iframe></a>
  </figure>

* Show the current configurations of the gamepads.
  <figure>
      <a href="/assets/images/gnome-games/gamepad-tester.webm"><iframe width="637" height="435" src="" frameborder="0" src="/assets/images/gnome-games/gamepad-tester.webm" allowfullscreen></iframe></a>
      <figcaption>It's so fun pressing random buttons on the controller and seeing the svg light up :P</figcaption>
  </figure>

* And finally remap them with this immersive UI
  <figure>
      <a href="/assets/images/gnome-games/gamepad-mapper.webm"><iframe width="637" height="435" src="" frameborder="0" src="/assets/images/gnome-games/gamepad-mapper.webm" allowfullscreen></iframe></a>
      <figcaption>Well it's hard to show here, but I remapped my buttons to triggers and vice versa (for an amazing Tekken 3 experience).</figcaption>
  </figure>
  In any case, if the users mess up, they can reset back to default mappings.

### Improvements

* Remapping the gamepad is a lengthy task, hence we plan on providing quick configurations for some common gamepads.
  As an example we could have a simple button swapper.

<figure class="half">
    <a href="/assets/images/gnome-games/button-swap-design-1.png"><img src="/assets/images/gnome-games/button-swap-design-1.png"></a>
    <a href="/assets/images/gnome-games/button-swap-design-2.png"><img src="/assets/images/gnome-games/button-swap-design-2.png"></a>
    <figcaption>Thanks Garett for the quick designs!</figcaption>
</figure>

* Adrien suggests that we should ask the users if they want to share their gamepad mappings online so that we can enhance the database.

---
