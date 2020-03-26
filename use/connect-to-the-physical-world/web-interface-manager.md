---
layout: doc
title: Web Interface Manager
permalink: /docs/use/connect-to-the-physical-world/web-interface-manager
---

## Web Interface for Managing Edge Servers

If you're just testing the Spatial Toolbox app on your device, you don't need to run any
additional servers or software. If, however, you want AR content to "stick" to images
or objects in the physical world, or if you want to program physical machines with these AR tools,
you need to run a Vuforia Spatial Edge Server from a computer in your network.

While you're running an edge server on your computer, it hosts a web application on port 8080
that you can use to configure various elements. Visit [localhost:8080](http://localhost:8080) on
your computer to view this web dashboard for your system.

## Managing Objects

The default view that opens when you visit localhost:8080 is the Object
Configuration view. This is where you can set up new *Objects* that will be hosted by this server,
connect them to Vuforia targets, manage the *Tools* attached to them, and view realtime
debugging information about their data points.
 
If you haven't created any objects yet on this server, a small interactive tutorial will appear
to walk you through the creation of the first one.

New objects start out as deactivated. In order to activate them, you need to upload Vuforia
target data that will attach this object to a visual object in the physical world. The easiest
option is to upload a JPG image for this object, but you can also make use of object or image
targets generated through the [Vuforia Developer Portal](http://developer.vuforia.com). 

You can toggle which objects are active and discoverable by Toolbox apps in this network by
clicking on the On/Off button for each object.

If you click on an active object's name, it will open a detailed debugging view for that object.
You'll be able to see live-updating values representing each of the nodes of any tools attached
to the object, and you can see a list of links involving these nodes.

## Managing Tools

Clicking on the "Spatial Tools" button near the top of the main page will switch to a new view,
where you can see all of the global tools supported by this server. These tools will get loaded
into the pocket of any Toolbox apps that are in the same network as this server. You can enable
or disable them by clicking the On/Off buttons.

If you're a developer and you add more tools to the system, you should check this view to see
that they have been properly loaded and enabled.

## Managing Hardware Interfaces

Clicking on "Manage Hardware Interfaces" will switch the the third view, where you can view,
enable, and configure any hardware interfaces that this server includes. Hardware interfaces
let the nodes of certain objects and tools read and write data to other hardware and connected
systems. By default, you will have a Kepware interface for connecting to industrial machinery,
but you may see more if you've installed any addons. You can click the On/Off button to enable
or disable any of these interfaces. Interfaces with a yellow gear icon have additional settings
that you can configure. Click on the yellow gear and edit the settings to customize the hardware
interface for you particular system.
