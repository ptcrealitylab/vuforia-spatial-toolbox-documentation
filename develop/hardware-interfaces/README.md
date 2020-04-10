---
layout: doc
title: Create New Interfaces
permalink: /docs/develop/hardware-interfaces
---

<a name="newInterface"></a>
## Create new Interfaces

Interfaces are plugins that allow you to connect any hardware or build out any service and connect it with the Spatial Toolbox. The Spatial Toolbox processes data via spatial nodes. They are the central data storing and data processing elements within the Spatial Toolbox. Therefore most of the API is focused on how to handle nodes and how to connect your Interface to the server.

#### Folder Structure
Your interface-addon should be stored in the addons/interfaces/[yourInterfaceAddon]/ folder, and it requires the following file:

- `index.js` this file defines the entry point for your spatial edge server interface.

Next: [Hardware Interface API Reference](./hardware-interfaces/api-reference) or [Hardware Interface
 Tutorial](./hardware-interfaces/tutorial).
