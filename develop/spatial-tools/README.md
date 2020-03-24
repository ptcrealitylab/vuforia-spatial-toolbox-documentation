---
layout: doc
title: Create New Tools
permalink: /docs/develop/spatial-tools
---

<a name="newTool"></a>
## Create New Tools

What is a spatial tool? It is like any other web application, just with a spatial context component to it. This means a plain HTML web application is located hovering in space, and a webGL application uses a full-screen web frame that can handle any webGL framework to position content in physical space.

How to get started? Create the following folderstructure within the vuforia-spatial-edge-server folder: ```/addons/<yourAddon>/tools/<newToolName>```. Place your spatial tool into this folder. Startpage is index.html

The following APIs will help you to configure your spatial Tool, allow the right visualization within the Spatial Toolbox, connect with other tools, and use the logic capabilities of the Spatial Toolbox. 


 Your tools will be available to all objects that are hosted from the server they are stored on. This means, if one of your objects is visible in the Spatial Toolbox, the Spatial Toolbox can make use of them.

Next: [Spatial Tools API Reference](./api-reference) or [Spatial Tools Tutorial](./tutorial).
