---
title: "My first android application"
excerpt: "And it got featured on GadgetHacks. Wait what?"

tags:
  - android
  - java
---

### EventLock

My first android application has been released, and it has reached 350 downloads[^1] in just two days.

<iframe width="560" height="315" src="https://www.youtube.com/embed/mPlSzRvHAFg" frameborder="0" allowfullscreen></iframe>
---

The video doesn't highlight the updated version, but can help you get the application up and running.

### Why?

* After Kitkat(Android 4.4), the lock screen widgets were disabled hence, calendar widgets cannot be displayed on the lock screen.
* Default lock screens for lollipop/marshmallow do not show calendar events either.
* Third party lock screens are heavy, have ads and privacy issues.

### Features

* See multiple events by scrolling
* Tons of customisations
* No advertisements
* No weird permissions(not even internet!)

### How to get it?

[Play Store](https://play.google.com/store/apps/details?id=com.gobbledygook.theawless.eventlock), [Xposed Repository](http://repo.xposed.info/module/com.gobbledygook.theawless.eventlock)

  This is a module for xposed installer and works only with root.  

### Screenshots (click to enlarge)

<figure class="third">
    <a href="https://raw.githubusercontent.com/theawless/EventLock/master/images/lockscreen.jpg"><img src="https://raw.githubusercontent.com/theawless/EventLock/master/images/lockscreen.jpg"></a>
    <a href="https://raw.githubusercontent.com/theawless/EventLock/master/images/presets.jpg"><img src="https://raw.githubusercontent.com/theawless/EventLock/master/images/presets.jpg"></a>
    <a href="https://raw.githubusercontent.com/theawless/EventLock/master/images/mainscreen.jpg"><img src="https://raw.githubusercontent.com/theawless/EventLock/master/images/mainscreen.jpg"></a>
</figure>

## Developer Stuff

Dragons ahead!

### How does it work?

[Github](https://github.com/theawless/EventLock)  

Xposed works in mysterious and magical ways, or so did I think.  
rovo89 the father of xposed has written [the ultimate guide](https://github.com/rovo89/XposedBridge/wiki/Development-tutorial) for making xposed applications.  
So how?
* Have an alarm receiver which starts the working service (when you ask? for that you have to read the code >_< )
* working service updates the event title and time by querying it from the calendar provider
* Get the lockscreen event from xposed and display the title and time

Simple stuff right?

### Attributions

  Here's what's helped me, and might help you if you create an xposed module.

* [QuoteLock](https://github.com/apsun/QuoteLock)
  Shows quotes on the lock screen.  
 Distributed under MIT license.

---
[^1]: Only on the xposed repository.
