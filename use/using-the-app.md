---
layout: doc
title: Getting Started
permalink: /docs/use/using-the-app
---

## Getting Started

## An illustrated guide to using the Vuforia Spatial Toolbox

Open the Vuforia Spatial Toolbox app and follow along to learn how to operate the user interface.

The examples here have a blank background for simplicity, but you will see all of these with a live camera stream in the background.

## Navigating the Menus

![intro to the menus](./images/ui-tutorial-gifs/01-intro-to-the-menus.gif)

There are five menu buttons always present on the right side of the screen. Learning what each does will guide you through the interface.

- The **Interfaces** button displays any AR *tools* placed into the space.
- The **Programming** button displays the programming *nodes* and *links* for any of the tools you are currently looking at.
- The **Pocket** button opens a scrolling menu of tools that you can add to the space.
- The **Settings** button opens the settings menu, where you toggle between different options and modes.
- The **Freeze** button is a useful feature to pause the video background into a still image, so the AR content doesn't move when you move your device.

## The Basics

### Adding Tools

![add slider and graph from pocket](./images/ui-tutorial-gifs/16-adding-tools-from-pocket.gif)

Tapping on an icon in the pocket will add that *tool* to the space.

You can then tap and hold on the tool for a moment (without moving your finger) to start moving it. Drag to move it horizontally and vertically. Pinch with two fingers to scale it up or down.

In this example, we add a slider tool and a graph tool, and organize them in our space.

### Drawing Links

![draw link from left node to right node](./images/ui-tutorial-gifs/02-create-link.gif)

When you switch to programming mode, you will see the *nodes* for any visible tools.

You can connect any two nodes by dragging a line between them.

This creates a *link* that will send data from the first node (in this example, on the left) to the second (on the right). The direction matters, so the animated dots move in the direction that data will flow.

### Cutting Links

![cut link by swiping through it](./images/ui-tutorial-gifs/03-delete-link.gif)

To delete a link, you can swipe through it to cut it.

The node on the left will no longer send data to the one on the right.

### Moving Nodes

![tap and hold on node and then drag it around](./images/ui-tutorial-gifs/04-move-node.gif)

If you tap and hold on a node for a second without moving your finger, you can then reposition it by dragging it around. Blue corners will appear around the node to show that you can move it.

The position doesn't affect the behavior, but it can be useful to keep the nodes organized.

### Interacting with Tools

![link and interact with slider and graph](./images/ui-tutorial-gifs/17-linking-tools.gif)

Some tools – like the graph – just visualize data. Others – like the slider – are inputs that we can interact with. By linking the input tool to the output tool, we can visualize the data that the input generates.

Before interacting with tools, remember to switch back from the programming mode to the interfaces mode.

### Fast-adding Tools

![add three tools by dragging not tapping](./images/ui-tutorial-gifs/19-adding-tools.gif)

Instead of tapping on a tool's icon in the pocket, if you tap-and-hold and *drag* the tool all in one gesture, you will be able to move and scale the tool all at once. This is just a shortcut if you want to quickly add tools to the space.

### Linking Multiple Tools

![string together three tools](./images/ui-tutorial-gifs/20-fixed-linking-tools.gif)

Each node functions both as an input and an output, so you can string together tools' nodes into longer programs.

In this example, the value from the slider is visualized first by a simple meter, and then gets passed to a time-series graph to be visualized in a different way.

### Deleting Tools

![delete tools by dragging to trash](./images/ui-tutorial-gifs/21-delete-tools.gif)

When dragging a tool around, a trash icon will appear on the right edge of the screen. Drag the tool on top of the icon and let go to delete it.

This will also delete all links to or from the nodes of this tool.

## Logic Nodes

Links just send data between nodes without affecting the data along the way. To add more logic to your system, you can add a *Logic Node* and pass the data through the logic node.

This part of the guide will teach you the basics of a block-based programming environment that lets you add all sorts of logic to your systems.

### Add Logic Node

![drag logic node from pocket icon](./images/ui-tutorial-gifs/05-add-logic-node.gif)

When you're in programming mode, dragging out from the pocket button will create a new logic node that you can place into space. There needs to be at least one tool in the space first, for the logic node to attach to, but you can add as many logic nodes as you want.

### Link to the Logic Node

![create links to and from logic node](./images/ui-tutorial-gifs/06-link-logic-node.gif)

Creating a link to a logic node lets you choose a color for it: blue, green, yellow, or red. You can also choose a color for links drawn out of a logic node. The color of this link determines which color-coded entry point it will connect to within the logic node.

In this example, we send the data from the slider into the blue input, and send data from the green output to the graph.

### Open the Logic Node

![tap on a logic node to open it](./images/ui-tutorial-gifs/07-open-logic-node.gif)

Tapping on a logic node opens its programming grid, where you can construct a program that affects the data flowing from its inputs to its outputs. You can place *logic block* into this grid, and link them together into programs.

The top row has an input spot for each color, corresponding to the colors of links connecting to this logic node.

The bottom row has an output spot for each color.

### Add Logic Block from Menu

![select and place inverter block from menu](./images/ui-tutorial-gifs/08-add-logic-block.gif)

Tapping on the pocket button within a logic node opens the logic menu, where you can see a set of logic blocks that you can choose from.

Tap down and drag a logic block to select it. Dragging it around will snap it into different grid spots that it gets close to. Let go of it while it is snapped onto a spot in order to place it.

In this example, we select an inverter block, and place it into the grid.

A full list of blocks, and how to use them, can be seen [here]().

### Link Blocks to Inputs and Outputs

![draw link from input to block to output](./images/ui-tutorial-gifs/09-link-logic-block.gif)

Drawing lines between blocks, input spots, or output spots will create a link. To create a working program, you need to make a path from an input to an output.

Since we connected the slider to the blue input of the logic node, and the graph to the green output, we link the blue input spot to the inverter block, and then link the block to the green output spot.

This will send inverted values from the slider to the graph.

### Move Logic Block

![move logic block to different spot](./images/ui-tutorial-gifs/10-move-logic-block.gif)

Tapping and holding on a block for a second will allow you to pick it up and move it to a different spot. The location you choose doesn't matter (unless you place it on an input or output spot) but it helps to organize them.

If you place it on an input spot, it will automatically link it to that input. If you place it on an output spot, it will automatically link it to that output.

### Logic Block Information

![open instructions for inverter block](./images/ui-tutorial-gifs/11-logic-block-settings.gif)

If you tap on a placed logic block (without holding), it will open the information and settings page for that block. For example, tapping on the inverter block explains what the block does. The inverter block doesn't have any settings that can be changed.

Tapping the back button closes the information. Tapping it again would exit the logic node entirely.

### Logic Block Settings

![add delay block and open settings](./images/ui-tutorial-gifs/12-logic-block-settings-delay.gif)

Some logic blocks have settings that you can change by tapping on them.

In this example, we add another block to the grid: a delay block that will output whatever data is sent to it after a certain amount of time. By tapping on the block, we can change that amount of time.

### Adjust Block Links

![link inverter to delay](./images/ui-tutorial-gifs/13-link-multiple-logic-blocks.gif)

A link between blocks can be deleted in a similar way as those between nodes: just swipe to cut the line.

Blocks can also be linked together, to compose their effects into a more complicated program.

In this example, the data from the blue input first gets inverted, and then gets sent into the delay block. After a few seconds, the delay block will send the inverted data to the green output. 

### Delete Blocks

![delete both blocks by dragging to trash](./images/ui-tutorial-gifs/14-delete-logic-blocks.gif)

When moving a block around, a trash icon will appear on the right edge of the screen. Drag the block onto that icon and let go to delete it (and all the links to or from that block).

### Delete Logic Node

![delete logic node by dragging to trash](./images/ui-tutorial-gifs/15-delete-logic-nodes.gif)

Logic nodes can be moved around by tapping and holding on them, just like regaular nodes. Regular logic nodes cannot be deleted, but logic nodes can. Drag a logic node onto the trash icon on the right edge of the screen to delete it and the links connected to it.

## Additional Services

The Vuforia Spatial Toolbox supports a variety of additional services that you can use to view and spatially interact with content in a variety of ways.

### Visibility Distance

One service included in your app is the ability to set from how far away a tool will be visible before it fades away. By default, tools will hide when you are more than 2 meters away from them, but this can be adjusted.

![adjust visibility distance](./images/ui-tutorial-gifs/27-adjust-visibility-distance.gif)

When holding on a tool so it can be repositioned, if you press and hold another finger on the green distance icon in the bottom right a blue sphere and dotted line will appear to show you how far this tool can be seen. As you walked towards or away from the tool, the size of the sphere will match your current distance. When you let go, it will set the visibility distance to your current distance. As you step further away, the tool will fade away, but it will reappear as you walk closer.

### Grouping

One service that isn't enabled by default, but can be useful, is grouping. If you're a developer, you can build new services like grouping and add them to the app using the addon system.

### Turning on Grouping Mode

![turn on grouping in settings](./images/ui-tutorial-gifs/22-enable-grouping.gif)

To enable the grouping service, open the settings menu and turn on the toggle switch for *Grouping*.

Grouping mode lets you form groups of tools that you can move together in space.

### Using the Grouping Lasso

![draw an empty grouping lasso on the background](./images/ui-tutorial-gifs/23-grouping-lasso.gif)

If grouping mode is turned on, double-tap on the background and draw a circle. This is your grouping lasso. Any tools inside this lasso will be grouped together when you let go.

### Grouping and Moving Tools

![group two tools together and move them](./images/ui-tutorial-gifs/24-group-and-move.gif)

In this example, we draw a circle around two tools to group them together. Now when we move one of them around it will also move the other relative to it.

To ungroup tools, draw a lasso around them again.

### Deleting a Group

![drag a group of tools to the trash](./images/ui-tutorial-gifs/25-delete-group.gif)

If you delete a tool in a group it will also delete all other tools in the same group. This can be a useful way to delete a lot of tools at once.

### Envelopes

Envelopes are a special type of tool that can contain other tools. They help us organize the space. Think of them as a way to put your other tools into boxes that you can open and close.

If you're a developer, you can build your own types of envelopes, but by default there is one envelope tool that you can use.

### Adding an Envelope

![add an envelope from the pocket](./images/ui-tutorial-gifs/28-adding-and-opening-envelope.gif)

This blue icon represents the envelope tool. Add one to your space and tap on it to open it. When it is open, you'll see a blue [X] icon in the top left corner.

### Adding Tools to an Envelope

![add two tools to an open envelope](./images/ui-tutorial-gifs/29-adding-frames-to-envelope.gif)

While you have an envelope open (you can see the [X] in the corner), all compatible tools that you add from the pocket will get added to that envelope. Here, we add two buttons to this envelope. When we press the [X] button, it closes the envelope. This hides all the tools that we put into it.

### Reopening an Envelope

![tap on envelope to show the tools inside it](./images/ui-tutorial-gifs/30-reopening-envelope.gif)

Tapping on an envelope reopens it, displaying all of the tools we put inside it.

### Multiple Envelopes

![tap on envelope to show the tools inside it](./images/ui-tutorial-gifs/31-opening-multiple-envelopes.gif)

You can only have one envelope open at a time. Tapping on another envelope when one is already open will close the first one before opening the second.

In this example, you can see how we use two envelopes to better organize a space with many tools.

Tools inside an envelope are not "grouped" like those using the grouping lasso – they do not move relative to one another. Grouping and envelopes are two different examples of services you can use to form different kinds of spatial relationships between the tools in your space.

## Developer Features

### Viewing Found Objects

![view list of found objects in settings](./images/ui-tutorial-gifs/26-found-objects.gif)

You may wish to see or debug which objects have been discovered by this client. To do so, open the settings menu and tap on the *Found Objects* button. It will open a page that shows an entry for each object discovered by this app.

You should always see an entry called "_WORLD_local", representing a local world object that your AR content will attach to by default. Additional entries will only appear if you are running additional Vuforia Spatial Edge Servers in your network.

Each object will display the IP address of the Edge Server it is being hosted by, and the list of tools that have been attached to it.

If an object name appears in red, that means that the Vuforia Engine was unable to initialize an AR target for that object using the data hosted by the Edge Server, so you won't be able to recognize that object in your space and see its AR content. Object names appearing in black have been successfully added to the AR tracker.
