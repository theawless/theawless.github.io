---
title: "GSoC Final Report"
excerpt: "Gamepad and Keyboard Configuration in Games"
header:
  teaser: "/assets/images/teasers/gnome-games.png"

tags:
  - GNOME
  - GSoC
  - GNOME Games
---

Google Summer of Code 2017 has come to an end, I worked on adding [Gamepad and Keyboard Configuration to GNOME Games](https://wiki.gnome.org/Outreach/SummerOfCode/2017/Projects/AbhinavSingh_GamesGamepadConfiguration). This post is a part of my final submission.


## Status

The project was divided into three phases. For details about the phases you can visit the previous reports - [1](https://theawless.github.io/GSoC-Report-1), [2](https://theawless.github.io/GSoC-Report-2), [3](https://theawless.github.io/GSoC-Report-3), [4](https://theawless.github.io/GSoC-Report-4).

I will brief about each phase here.

### Gamepad Configuration

The heart of my project. Thankfully, we were able to push it to master. With this feature in place, users will be able to create mappings for their gamepads. It serves two purposes:

* Allow users to create mappings for exotic gamepads which do not have a mapping in SDL repository.

* Allow the possibility of mapping various inputs to other inputs. For example, if the user wants to press Button A and have the effect of Button B.
  My code will not be directly used for this purpose (as sdl mappings are global, and this kind of configuration will be per game/per player basis), but it will provide a great starting point.

Currently, the mapping procedure takes time, and an improvement would be to allow quick configurations like flipping buttons or triggers.

### Controller Reassignment

The goal of this phase was to let users to reorder their controllers. Previously, the users had no control of which controller will be assigned to which player.

My current code for this feature works well, barring the edge cases (which I have discussed in [4](https://theawless.github.io/GSoC-Report-4)). Some work will be needed to make it master-ready. Many improvements can be made for the UI design, and I will need input from the pros.

### Keyboard Configuration

Much like the first phase, this phase aimed to allow users to configure their keyboard.

Gamepad Configuration took more time than what I had expected. The good news is that its code can be reused for this phase.
I have already finished this phase, you can build it, and see that it works (more or less). Still, it needs reviewing from my mentor to make sure that I haven't missed anything.

A stretch goal would be to allow for one-click configurations like changing direction keys from arrows to ASWD or numpad.

### Bug fixes

I was able to fix a few bugs here and there along with the project. Most of them being related to the recent port of the gamepad module to C, and one on making Games look better on custom themes.


## Code

All of my code can be found on GNOME's Git instance and Bugzilla. Additionally, I have uploaded all the patches to a Google Drive [folder](https://drive.google.com/open?id=0Bx3DIM7wGNgocTVweGwzVTNPQTQ).

| Task | Git | Bugzilla | Patches |
| --- | --- | --- | --- |
| Gamepad Configuration | [gamepad-config](https://git.gnome.org/browse/gnome-games/log/?h=wip/abhinavsingh/gamepad-config) | [780754](http://bugzilla.gnome.org/show_bug.cgi?id=780754) | 22 |
| Controller Reassignment | [gamepad-reassign](https://git.gnome.org/browse/gnome-games/log/?h=wip/abhinavsingh/gamepad-reassign) | [780756](http://bugzilla.gnome.org/show_bug.cgi?id=780756) | 6 |
| Keyboard Configuration | [keyboard-config](https://git.gnome.org/browse/gnome-games/log/?h=wip/abhinavsingh/gamepad-config) | [780755](http://bugzilla.gnome.org/show_bug.cgi?id=780755) | 6 |
| Bug fixes (since May) | [master](https://git.gnome.org/browse/gnome-games/log/) | [780714](http://bugzilla.gnome.org/show_bug.cgi?id=780714), [782549](http://bugzilla.gnome.org/show_bug.cgi?id=782549), [779128](http://bugzilla.gnome.org/show_bug.cgi?id=779128) | 5 |


## Thank you

I've had a wonderful time at GNOME since I began contributing in January, and Google made it more fun and certainly more profitable ;)

I am grateful that I got to work with such a great community. It was a pleasure working under my mentor [Adrien Plazas](https://wiki.gnome.org/AdrienPlazas). I would surely work with him in the future to make Games the hottest application out there.

Thank you GNOME, Google, Adrien and, Suhas for making my summers super exciting!

---
