# Gtk+ and status icons

## Introduction

Since version 2.10 `gtk+` provides support for so-called _status icons_, according to freedesktop.org or `win32 api` guidelines.

_Status icon_ is a small image displayed in notification area (another name: _system tray_). Left mouse click on the status icon should invoke a default action whereas right click should popup a contextual menu.

_Status icon_ object is very easy to handle: it should be created and shown — it is immediately visibile in the notification area.

_Status icon_ object can handle `activate` signal (emitted when user clicked left mouse button) and `popup-menu` signal (emitted when user clicked right mouse button)

The icon can also blink and it works fine either in X11 notification area or win32 system tray.

## Theory and examples

Status icon object can be created using the default constructor:


```python
statusIcon = gtk.StatusIcon()
```

By default, when the object is shown, only the rectangle occupied by the icon is visibile, so it is a good practice to display any image:

```python
statusIcon.set_from_stock(gtk.STOCK_HOME)
```

`StatusIcon` class offers few methods to set an image (it is possible to load stock icon, pass a filename with a valid image or set an image loaded previously elsewhere)

`StatusIcon` class also allows to set a tooltip for the icon:

```python
statusIcon.set_tooltip(„Tooltip text”)
```

In cause of an urgent event the icon in notification area can blink:

```python
statusIcon.set_blinking(blinking)
```

The blinking parameter orders the icon to blink or to stop blinking.

### Working examples

Following example illustrates simple StatusIcon creation and management, This code should be very easy to analyze:

```python
#!/usr/bin/env python

import gtk
import pygtk

def quit_cb(widget, data = None):
    if data:
        data.set_visible(False)
    gtk.main_quit()

def popup_menu_cb(widget, button, time, data = None):
    if button == 3:
        if data:
            data.show_all()
            data.popup(None, None, None, 3, time)
    pass

def activate_icon_cb(widget, data = None):
    msgBox = gtk.MessageDialog(parent = None, buttons = gtk.BUTTONS_OK, message_format = „StatusIcon test.”)
    msgBox.run()
    msgBox.destroy()

statusIcon = gtk.StatusIcon()

menu = gtk.Menu()
menuItem = gtk.ImageMenuItem(gtk.STOCK_ABOUT)
menuItem.connect(‚activate’, activate_icon_cb)
menu.append(menuItem)
menuItem = gtk.ImageMenuItem(gtk.STOCK_QUIT)
menuItem.connect(‚activate’, quit_cb, statusIcon)
menu.append(menuItem)

statusIcon.set_from_stock(gtk.STOCK_HOME)
statusIcon.set_tooltip(„StatusIcon test”)
statusIcon.connect(‚activate’, activate_icon_cb)
statusIcon.connect(‚popup-menu’, popup_menu_cb, menu)
statusIcon.set_visible(True)

gtk.main()
```

The next example demonstrates blinking of the icon:

```python
#!/usr/bin/env python

import gtk
import pygtk

def delete_cb(widget, event, data = None):
    if data:
        data.set_blinking(True)
    return False

def quit_cb(widget, data = None):
    if data:
        data.set_visible(False)
    gtk.main_quit()

def popup_menu_cb(widget, button, time, data = None):
    if button == 3:
        if data:
            data.show_all()
            data.popup(None, None, None, 3, time)
    pass

def activate_icon_cb(widget, data = None):
    if data:
        data.set_blinking(False)
    window.show_all()
    window.set_urgency_hint(True)

statusIcon = gtk.StatusIcon()

window = gtk.Window(gtk.WINDOW_TOPLEVEL)
window.set_title(„StatusIcon test (II)”)
window.connect(‚delete_event’, delete_cb, statusIcon)

menu = gtk.Menu()
menuItem = gtk.ImageMenuItem(gtk.STOCK_ABOUT)
menuItem.connect(‚activate’, activate_icon_cb, statusIcon)
menu.append(menuItem)
menuItem = gtk.ImageMenuItem(gtk.STOCK_QUIT)
menuItem.connect(‚activate’, quit_cb, statusIcon)
menu.append(menuItem)

statusIcon.set_from_stock(gtk.STOCK_HOME)
statusIcon.set_tooltip(„StatusIcon test (II)”)
statusIcon.connect(‚activate’, activate_icon_cb, statusIcon)
statusIcon.connect(‚popup-menu’, popup_menu_cb, menu)
statusIcon.set_blinking(True)
statusIcon.set_visible(True)

gtk.main()
```