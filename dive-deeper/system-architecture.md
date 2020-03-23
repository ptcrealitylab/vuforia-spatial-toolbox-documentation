---
layout: doc
title: System Architecture
permalink: /docs/dive-deeper/system-architecture
---

# System Architecture

This is meant to be a overview of the system architecture that will answer the
following questions:

 1. What are the overall components of the system?
 2. What are the components and responsibilities of the client and server?
 3. What does a deployment of the Vuforia Spatial Toolbox look like as a network?
 
## Overview

Vuforia Spatial Toolbox at its simplest consists of a single Vuforia Spatial Toolbox iOS or
Android client. This client will spin up (using Node.js) a Vuforia Spatial Edge Server upon
launching. The client loads certain capabilities, such as which types of AR content it can
place into space, from the local Edge Server. Metadata about the AR content added by the
client, such as where it is positioned and whether its data is linked to any other content,
gets saved in the Edge Server rather than the client. The data programming engine and all
resulting data processing happens in the Edge Server.
     
The system is scalable in the number of clients and the number of servers connected in the same
network. Additional Edge Servers can be spun up from different computers in the local WiFi
network, and all clients connected to the same network will be able to discover and make
changes to content from those servers.
   
The local Edge Server running from a client device does not persist data from session to
session, and is not visible to other clients. All other Edge Servers are visible across the
network and persist content and spatial programming.
  
There is currently no cloud connectivity in the system. The security of your deployment is
largely defined by the security of your WiFi network.

## Client Architecture

A user interacts with the system using either the
[Vuforia Spatial Toolbox iOS](https://github.com/ptcrealitylab/vuforia-spatial-toolbox-ios)
application, or the Vuforia Spatial Toolbox Android application (coming soon).

The iOS application consists of an Xcode project with four primary responsibilities:
 1. Loads the
    [Vuforia Spatial Toolbox User Interface](https://github.com/ptcrealitylab/vuforia-spatial-toolbox-userinterface)
    into a WebView, which will render the entirety of the GUI and handle user interactions.
 2. Renders a camera feed in the background, and runs the Vuforia Engine SDK to detect the presence
    and position of any trackable targets (image targets, object targets, etc).
 3. Provides an API for the User Interface WebView to interact with native device APIs
    and capabilities, such as listening for UDP messages or adding targets to Vuforia Engine. A
    full list of the supported APIs can be found in the
    [JavaScriptAPIHandler.h](https://github.com/ptcrealitylab/vuforia-spatial-toolbox-ios/blob/master/Vuforia%20Spatial%20Toolbox/JavaScriptAPIHandler.h)
    header file.
 4. Creates a Node.js thread and starts a local instance of the Vuforia Spatial Edge Server, which
    provides default capabilities to the client in the absence of other Edge Servers in the local
    WiFi network. The Local Edge Server also serves the User Interface to the WebView.

## Server Architecture

Each Vuforia Spatial Edge Server is a Node.js application that provides the following capabilities:

 1. Broadcasts its existence with UDP multicasts so that clients and other edge
    servers can automatically discover this edge server and its contents.
 2. Hosts an HTTP web server to provide REST and WebSocket interfaces to communicate
    with clients and other edge servers.
 3. Contains a set of
    [Objects](https://github.com/ptcrealitylab/vuforia-spatial-toolbox-documentation/blob/master/understandSystem/dataModel.md#object)
    that map AR content and metadata to Vuforia Targets stored on this server, and provides methods
    to locally persist changes to this data.
 4. Runs an Object Engine, which processes the data for
    [Nodes](https://github.com/ptcrealitylab/vuforia-spatial-toolbox-documentation/blob/master/understandSystem/dataModel.md#node)
    and
    [Logic Nodes](https://github.com/ptcrealitylab/vuforia-spatial-toolbox-documentation/blob/master/understandSystem/dataModel.md#logic-node)
    and propagates the data across
    [Links](https://github.com/ptcrealitylab/vuforia-spatial-toolbox-documentation/blob/master/understandSystem/dataModel.md#link)
    to Objects on this server, or to objects on other known Edge Servers using WebSockets.
 5. Loads an extendable set of modules from its addons directory, including the default
    [core-addon](https://github.com/ptcrealitylab/vuforia-spatial-core-addon),
    which provide different types of nodes,
    [blocks](https://github.com/ptcrealitylab/vuforia-spatial-toolbox-documentation/blob/master/understandSystem/dataModel.md#block),
    and
    [tools](https://github.com/ptcrealitylab/vuforia-spatial-toolbox-documentation/blob/master/understandSystem/dataModel.md#frame-tool)
    that the objects on this server can utilize, as well as (hardware) interfaces. Interfaces
    allow the objects on this server to read and write data to external systems, such as using
    kepware to connect to industrial IoT systems.
 6. Serves a web dashboard on http://localhost:8080 where a system admin can manage the objects,
    tools, and interfaces on this server.
 
A client's Local Edge Server (the server running on the mobile device) is identical to any other
Edge Server except it only runs a subset of its capabilities. It does not advertise its
existence to other servers, it does not save data persistently between sessions, it does not
connect with hardware interfaces, and creates exactly one object upon initialization (called the
Local World Object) rather than having the ability to create custom objects through the web
dashboard.

## Network Topology

A Vuforia Spatial Toolbox deployment is currently limited to the local WiFi network in which it
runs. This network must allow UDP multicasting for the clients and servers to auto-discover one
another (although there is some experimental work to manually specify the IP of a server
if running in a network that does not support multicasting).
  
A network may have as few as one edge server. This server can host zero, one, or many objects.
There can also be many servers running simultaneously in the same network. Depending on your
preference for decentralizing or centralizing your objects and computation, you may have many
servers distributed in your space that each host a single object, or you may have a single
server that hosts all of your objects, or any combination between those extremes.

Servers establish point-to-point WebSocket communication upon discovering one another, so if
you distribute your system across multiple edge servers there will be no central point where all
data passes through. For example, if an object on server A is linked to an object on server B,
only server A and B are involved in the resulting communication, so the network does *not*
become exponentially more congested as more servers are added. The scalability of the system
exceeds the size of all networks that we have currently been able to test it with.

## Discussions

If you still have any questions about the system architecture, please ask them on our
[forum](https://forum.spatialtoolbox.vuforia.com), so we can update this documentation to make it
even more complete.
