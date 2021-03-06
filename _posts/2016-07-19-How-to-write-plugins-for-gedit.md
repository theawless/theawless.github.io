---
title: "How to write plugins for gedit"
excerpt: "Ever wondered how to write plugins for the awesome GNOME text editor?"
header:
  image: /assets/images/splashes/gedit.png

tags:
  - gedit
  - python
  - GNOME
---

This is a comprehensive guide on writing plugins for gedit in Python3.

### Requirements

* [gedit](https://wiki.gnome.org/Apps/Gedit)
* Python3

### Structure

* `name.plugin`
  It tells gedit where the plugin is found. It contains author's details. This file is in .desktop format.

* `name.py` or `name/__init.py__`
  This file contains the actual code. It can also be a python package.

Both files are placed in either `/usr/lib/gedit/plugins/` (system-wide) or `~/.local/share/gedit/plugins/` (single-user).

### gedit plugin interfaces

This section will try to explain how gedit plugins work.

A Python plugin will be able to have one or more extensions. Each extension is derived from GObject.Object and must implement one of the interfaces that gedit provides for the extension points. These define the entry points in your code.

All extension points will have the defined gedit object as a property.

> Example: Extension point from gedit.AppActivatable will have `app` property and hence would be able to use Gedit.App API.

Refer to the following list to know which interface should be used and when:

* Gedit.AppActivatable : App

  Provides `do_activate`, `do_deactivate`.  
  gedit usually only runs one instance, even if you launch it several times hence, you can get application menu extension, or can initialize singleton objects using this interface.
  
* Gedit.WindowActivatable : Window

  Provides `do_activate`, `do_deactivate`, `do_update_state`.  
  Should be used when you want to change/delete/create tabs, to access bottom bar, etc.

* Gedit.ViewActivatable : View

  Provides `do_activate`, `do_deactivate`, `do_update_state`.  
  When operations on all views are required. Like removing trailing whitespaces on document save. 

`do_activate` is called in the extension point when gedit object of its defined type is created.  

`do_deactivate` is called in the extension point when gedit object of its defined type is destroyed.  

`do_update_state` is called in the extension point when gedit object of its defined type needs a UI update.  

> Example: `Gedit.WindowActivatable.do_activate` is called when a window object is created.

When a plugin is activated, all "activate" methods from the extension points will be called, and when the plugin is deactivated the "deactivate" methods from all extension points will be called.

### Example Plugin

Let us create an example plugin which instantly clears the document.

* Plugin File

  <figure>
      <a href="https://raw.githubusercontent.com/theawless/Clear-Doc/master/images/about.png"><img src="https://raw.githubusercontent.com/theawless/Clear-Doc/master/images/about.png"></a>
  </figure>

  ```
[Plugin]
Loader=python3
Module=example
IAge=3
Name=Example
Icon=example
Description=An example plugin which clears the document.
Authors= theawless <theawless@gmail.com>
Copyright=Copyright © 2016 Abhinav Singh <theawless@gmail.com>
Website=https://github.com/theawless/Clear-Doc
```

  Most of the fields are pretty obvious in the .plugin file. Here are some tips

    * `IAge` is always 3.
    * `Module` does __not__ have `.py` extension. And you can also specify a package here.
    * `Icon`

      You should install your icon in `$prefix/share/icons/hicolor/scalable/apps` and then update the icon cache using command `gtk-update-icon-cache-3.0`.  
Read more about icon specification [here](https://specifications.freedesktop.org/icon-theme-spec/icon-theme-spec-latest.html#install_icons).

* Imports

  PeasGtk for implementing settings -- more on this later  
  Gio for menu  
  Gtk is the gnome's UI library  
  Gedit for gedit's API functions  
  GObject : Fundamental object. Many gnome libraries like Gtk, Pango inherit from it  

  ```python
from gi.repository import GObject, Gtk, Gedit, PeasGtk, Gio
```

* Activatable

  Let's define a basic Activatable.

  ```python
class ExampleAppActivatable(GObject.Object, Gedit.AppActivatable):
    app = GObject.property(type=Gedit.App)
    __gtype_name__ = "ExampleAppActivatable"

    def __init__(self):
        GObject.Object.__init__(self)

    def do_activate(self):
        pass

    def do_deactivate(self):
        pass
```

* Menu

  <figure>
      <a href="https://raw.githubusercontent.com/theawless/Clear-Doc/master/images/menu1.png"><img src="https://raw.githubusercontent.com/theawless/Clear-Doc/master/images/menu1.png"></a>
      <a href="https://raw.githubusercontent.com/theawless/Clear-Doc/master/images/menu2.png"><img src="https://raw.githubusercontent.com/theawless/Clear-Doc/master/images/menu2.png"></a>
  </figure>

  Gedit.AppActivatable: We need to create an AppActivatable class first. We insert the menu to the main menu and set accelerators(keyboard shortcuts). Real action is defined later. Menu creation should be done in `do_activate` and deletion should be done in `do_deactivate`. You should read about [Gio.Menu](https://developer.gnome.org/gnome-devel-demos/stable/gmenu.py.html.en).

  ```python
    #call me in do_activate
    def _build_menu(self):
        # Get the extension from tools menu        
        self.menu_ext = self.extend_menu("tools-section")
        # This is the submenu which is added to a menu item and then inserted in tools menu.        
        menu = Gio.Menu()
        sub_menu_item = Gio.MenuItem.new("Clear Document", 'win.clear_document')
        menu.append_item(sub_menu_item)
        self.menu_item = Gio.MenuItem.new_submenu("Example", menu)
        self.menu_ext.append_menu_item(self.menu_item)
        # Setting accelerators, now our action is called when Ctrl+Alt+1 is pressed.
        self.app.set_accels_for_action("win.clear_document", ("<Primary><Alt>1", None))

    #call me in do_deactivate
    def _remove_menu(self):
        # removing accelerator and destroying menu items
        self.app.set_accels_for_action("win.dictonator_start", ())
        self.menu_ext = None
        self.menu_item = None
```
  Gedit.WindowActivatable: We need to create a WindowActivatable class too. Here we connect to the menu and define the action of clearing the document. We will connect the action to a callback function. That's how [signals](http://python-gtk-3-tutorial.readthedocs.io/en/latest/basics.html) (similar to events) are used in Gtk. As soon as `activate` is fired, `action_cb` is called. It's now clear why we used WindowActivatable. It has the power to give us the active document so that we can clear it. 

  ```python
    #call me in do_activate
    def _connect_menu(self):
        action = Gio.SimpleAction(name='clear_document')
        action.connect('activate', self.action_cb)
        self.window.add_action(action)

    def action_cb(self, action, data):
        # On action clear the document.
        doc = self.window.get_active_document()
        doc.set_text("")

    # this is called every time the gui is updated
    def do_update_state(self):
        # if there is no document in sight, we disable the action, so we don't get NoneException
        if self.window.get_active_view() is not None:
            self.window.lookup_action('clear_document').set_enabled(true)
```
  However weird it may seem; here, gui for menu is created in Gedit.AppActivatable while action is defined in Gedit.WindowActivatable. Gtk supports app-menu(which is available for all windows) and window-menu(which is defined for each window). Now because gedit is a single window application, but it creates the menu in appmenu, we have to first create AppActivatable. It may be a little hard to understand for those new to Gtk.


* Bottom Panel/Side Panel

  <figure>
      <a href="https://raw.githubusercontent.com/theawless/Clear-Doc/master/images/bottom-bar.png"><img src="https://raw.githubusercontent.com/theawless/Clear-Doc/master/images/bottom-bar.png"></a>
  </figure>

  The bottom panel is a [Gtk.Stack](https://developer.gnome.org/gtk3/stable/GtkStack.html). You will need to learn to read the C documentation for Gnome Libraries, because Python Documentation is out-dated and incomplete. Addition of bottom panel should also be done in `do_activate`.

  ```python
    #call me in do_activate
    def _insert_bottom_panel(self):
        # Add elements to panel.
        self.bottom_bar.add(Gtk.Label("Hello There!"))
        # Get bottom bar (A Gtk.Stack) and add our bar.        
        panel = self.window.get_bottom_panel()
        panel.add_titled(self.bottom_bar, 'example', "Example")
        # Make sure everything shows up.
        panel.show()
        self.bottom_bar.show_all()
        panel.set_visible_child(self.bottom_bar)

    #call me in do_deactivate
    def _remove_bottom_panel(self):
        panel = self.window.get_bottom_panel()
        panel.remove(self.bottom_bar)
```
* Configuration Dialog

  <figure>
      <a href="https://raw.githubusercontent.com/theawless/Clear-Doc/master/images/settings.png"><img src="https://raw.githubusercontent.com/theawless/Clear-Doc/master/images/settings.png"></a>
  </figure>

  Let's implement the configurable dialog, it can be used to change plugin settings.
  First we need to inherit from PeasGtk.Configurable

  ```python
class ExampleWindowActivatable(GObject.Object, Gedit.WindowActivatable, PeasGtk.Configurable):
    window = GObject.property(type=Gedit.Window)
    __gtype_name__ = "ExampleWindowActivatable"

    def __init__(self):
        GObject.Object.__init__(self)

    def do_create_configure_widget(self):
        # Just return your box, PeasGtk will automatically pack it into a box and show it.
        box=Gtk.Box()
        box.add(Gtk.Label("Settings Here!"))
        return box
```

### Source Code

Get the full source code of the [Example Plugin](https://github.com/theawless/Clear-Doc).  
You can also visit [Dict'O'nator](https://github.com/theawless/Dict-O-nator), a dictation plugin I wrote for gedit.

### Reference Pages

* [Gedit API](https://developer.gnome.org/gedit/stable/)
* [C Gtk3+ API](https://developer.gnome.org/gtk3/stable/)
* [Python3 Gtk3+ API](http://python-gtk-3-tutorial.readthedocs.org/)


