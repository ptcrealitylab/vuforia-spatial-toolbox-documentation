<a name="intro"></a>
# Introduction

<!---<img align="right" width="150" height="400" src="folder.svg">--->

To build new tools, you need to know a few basics:

1. All data is stored, processed, and handled via nodes.
2. Nodes are little programs that live in the Spatial Edge Server. Some are visible to the user in physical space, and some are hidden and only serve as data endpoints. Nodes can represent the physical functionality of a real-world object or just a virtual functionality of a spatial tool.
3. No matter what functionality a node has, they always belong to a Tool, and Tools are attached to objects in space.
4. Node templates are stored on the server and instantiated into your Tool every time a new Tool is placed into the world.

The following APIs will help you to define how a spatial tool behaves, how it connects to the Edge Server, and how it instantiates the nodes.

<!--- <br clear="right"/> --->

### Index
*  [Create new Tools](#newTool)
	- [API Reference](#toolApi)
		- [Initialize the Javascript Library within your Spatial Tool](#init)
		- [Communication with other Spatial Tools](#comm)
		- [Subscribe to Matrices](#subscribe)
		- [Control AR Screen Position](#control)
		- [Media Content](#media)
		- [Tool Behavior](#behavior)
		- [Spatial Edge Server Communication](#edgeComm)
*  [Create new Interfaces](#newInterface)
	- [API Reference](#interfaceApi)
*  [Create new Nodes](#newNode)
	- [index.js](#nodeIndex)
	- [gui/index.html](#nodeGuiIndex)
*  [Create new Blocks](#newBlock)
	- [index.js](#blockIndex)
	- [gui/index.html](#blockGuiIndex)
	- [gui/icon.svg](#blockGuiIcon)
	- [gui/label.svg](#blockGuiLabel)
