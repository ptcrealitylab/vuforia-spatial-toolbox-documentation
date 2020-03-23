---
layout: doc
title: API Reference
permalink: /docs/hardware-interfaces/api-reference
---

<a name="interfaceApi"></a>
### API Reference

##### Initialize the interface

```js
//Enable this hardware interface
var server = require('@libraries/hardwareInterfaces');
var settings = server.loadHardwareInterface(__dirname);

exports.enabled = settings('enabled');
exports.configurable = true; // can be turned on/off/adjusted from the web frontend

if (exports.enabled) {

// place your interface code here

}

```

##### loadHardwareInterface(`_dirName`)
- `_dirName` _(String)_ Must use string `_dirName` and returns the path to executed file
- **Returns** _(Object)_ Returns content of the settings json file stored for this interface

Returns access to the stored settings for this Interface

```javascript
var settings = server.loadHardwareInterface(__dirname);
```

##### addNode(object, tool, node, type, position) 
- `object` _(String)_ Name of Object
- `tool` _(String)_ Name of Tool
- `node ` _(String)_ Name of Node
- `type ` _(String)_ Name of the node Type. The server needs to have a node of this type; otherwise, it will use a default type.
- `position ` _(Object)_ [Optional] {x: float, y: float} object for the node's starting position. otherwise random.

Adds a new node to a Tool. If the Tool does not exist, it automatically generates a default tool with the `tool`-name. If the Object does not exist, it automatically creates a default object with the `object`-name. This function should be the first you call to initialize all required nodes.

```javascript
server.addNode("feederStation", "scale", "scaleValue", "node", {x: 1, y: 1})
```


##### renameNode(object, tool, oldNode, newNode)
- `object` _(String)_ Name of Object
- `tool` _(String)_ Name of Tool
- `oldNode ` _(String)_ Name of existing Node
- `newNode ` _(String)_ new name for the existing Node

Rename an existing node


```javascript
server.renameNode("feederStation", "scale", "scaleValue", "measurement");
```

##### moveNode(object, tool, node, x, y, scale, matrix, loyalty) 
- `object` _(String)_ Name of Object
- `tool` _(String)_ Name of Tool
- `node ` _(String)_ Name of Node
- `x` _(String)_ new x coordinate relative to the node matrix transform
- `y` _(String)_ new y coordinate relative to the node matrix transform
- `scale` _(String)_ new scale relative to the node matrix transform
- `matrix` _(Number[16])_ new matrix transform
- `loyalty` _(String)_ define the loyalty of Object (grounPlane, Object, world, ...)


Move a node to a new possition.

```javascript
server.moveNode("feederStation", "scale", "measurement", 1, 1, 1, identityMatrix, "object"); 
```

##### removeNode(object, tool, node)
- `object` _(String)_ Name of Object
- `tool` _(String)_ Name of Tool
- `node ` _(String)_ Name of Node

Remove node from a Tool.

```javascript
server.removeNode("feederStation", "scale", "measurement");
```

##### attachNodeToGroundPlane(object, tool, node, shouldAttachToGroundPlane)
- `object` _(String)_ Name of Object
- `tool` _(String)_ Name of Tool
- `node ` _(String)_ Name of Node
- `shouldAttachToGroundPlane ` _(Boolean)_ Define if Node is attached to the grounPlane

Lets you attach your Node to the ground plane. This is similar to loyalty, and eventually, an API around loyalty will replace this functionality.

```javascript
server.attachNodeToGroundPlane("feederStation", "scale", "measurement", true)
```


##### clearObject(object, tool)
- `object` _(String)_ Name of Object
- `tool` _(String)_ Name of Tool

In case your Object handles a lot of changes to the nodes, use this function to clear the memory of unused nodes. You must call this function after you have initialized all your nodes. Otherwise, you lose data.


```javascript
server.clearObject("feederStation", "scale");
```

##### removeAllNodes(objectName, tool)
- `object` _(String)_ Name of Object
- `tool` _(String)_ Name of Tool

Remove all nodes from your Tool.

```javascript
server.removeAllNodes("feederStation", "scale");
```

##### reloadNodeUI(objectName)
- `object` _(String)_ Name of Object

Force every Spatial Toolbox that has access to your spatial Tool to refresh its data about your Spatial Tool.

```javascript
server.reloadNodeUI("feederStation");
```


##### write(object, tool, node, value, mode, unit, unitMin, unitMax)
- `object` _(String)_ Name of Object
- `tool` _(String)_ Name of Tool
- `node ` _(String)_ Name of Node
- `value ` _(Number)_ The new value for the Node. The range <u>must</u> be between 0.0 and 1.0.
- `mode ` _(String)_ [optional] Data mode. Currently, `f` for float is the only supported choice.
- `unit ` _(String)_ [optional] The unit of your data such as 'kg', 'minutes', 'liter',... 
- `unitMin ` _(String)_ [optional] value range minimum
- `unitMax ` _(String)_ [optional] data range maximum

Write flow data to a node owned by the Spatial Tool within your Object. This flow data is of a simple floating data type with values between 0.0 and 1.0. The example below, writes 0.5 to the `node` = "nodeName", `unit` = kg, `min` = 0, `max` = 10 and the last entry tells that the value is only updated on change. The resulting Value will be 5 kg.
 
```javascript
server.write("feederStation", "scale", "measurement", 0.5, "f", "kg", 0, 10);
```

##### addReadListener(object, tool, node, callBack)
- `object` _(String)_ Name of Object
- `tool` _(String)_ Name of Tool
- `node ` _(String)_ Name of Node
- `callback ` _(Function)_ 
- **Returns** flowDataObject _(Object)_ flow data object for the Node

Add a read listener to read value changes from a node.

```javascript
server.addReadListener("feederStation", "scale", "measurement", function(flowDataObject){
    var value = flowDataObject.value;
    var unit = flowDataObject.unit;
});

```

##### removeReadListeners(object, tool)
- `object` _(String)_ Name of Object
- `tool` _(String)_ Name of Tool

It removes all ReadListeners currently running for a tool.

```javascript
server.addReadListener("feederStation", "scale");

```

##### map(x, in_min, in_max, out_min, out_max)
- `x` _(Number)_ Input Value
- `in_min` _(Number)_ Input Minimum
- `in_max` _(Number)_ Input Maximum
- `out_min` _(Number)_ Output Minimum
- `out_max` _(Number)_ Output Maximum
- **Returns** _(Object)_ Result

It scales an input value to the scope of an output value. Use this function to scale the data flow scale of 0.0 - 1.0 to the scale of your units.

```javascript
var scaleValue = 0.5
var outputValue = server.addReadListener(scaleValue, 0.0, 1.0, 0.0, 10.0);
```

##### writePublicData(object, tool, node, dataObject, data) 
- `object` _(String)_ Name of Object
- `tool` _(String)_ Name of Tool
- `node ` _(String)_ Name of Node
- `dataObject` _(String)_ Key to select a dataObject within public Data
- `data ` _(Object/String/Number)_ Data you want to store with the key

PublicData is a JSON object that contains data objects specified by the node definition. You need to finetune your Tool via specific node types. publicData allows you to handle complex data among the server-side node program and the Spatial Tool. The Value can contain any JSON encoded data.

```javascript
spatialTool. writePublicData("feederStation", "scale", "measurement", "lastMeasurements", [0, 1, 0.1, 0, 2]);
```

##### addPublicDataListener(object, tool, node, dataObject, callBack) 
- `object` _(String)_ Name of Object
- `tool` _(String)_ Name of Tool
- `node ` _(String)_ Name of Node
- `dataObject ` _(String)_ This is a key to select an object within publicData.
- `callback ` _(Function)_
- **Returns** value _(Object/String/Number)_ returns the stored data.

Add a read listener for publicData to read value changes for a defined Key. These changes are synchronized with the Edge Server and any Spatial Toolbox interacting with the Tool.

```javascript
spatialTool.addReadPublicDataListener("feederStation", "scale", "measurement", "lastMeasurements", function(value){
    var publicDataValue = value;
});
```

##### addConnectionListener(object, tool, node, callBack)
- `object` _(String)_ Name of Object
- `tool` _(String)_ Name of Tool
- `node ` _(String)_ Name of Node
- `callback ` _(Function)_
- **Returns** link _(Object)_ returns a link object

Listen for when there is a new link associated with the Node.
 
```javascript
server.addConnectionListener("feederStation", "scale", "measurement", function(link){
    var newLink = link;
});
```

##### getAllObjects()
- **Returns** _(Object)_ returns all objects.

Returns a pointer to all known objects

```javascript
server.getAllObjects();
```

##### getKnownObjects()
- **Returns** _(Object)_ returns all known objects in the format `{uuid : {version:"", protocol:"", ip:""}, ...}`

Get all known objects among the entire known network.

```javascript
var knownObjects = server.getKnownObjects();
```

##### getAllFrames(object) // should be renamed to getAllTools
- `object` _(String)_ Name of Object
- **Returns** _(Object)_ returns an object with all tools for a single object

Return all tools attached to an object. 

```javascript
var allTools = server.getAllFrames("feederStation");
```

##### getAllNodes(object, tool)
- `object` _(String)_ Name of Object
- `tool` _(String)_ Name of Tool
- **Returns** _(Object)_ returns an object with all nodes for a single tool

Return all nodes attached to a tool. 

```javascript
var allNodesForTool = server.getAllNodes("feederStation", "scale");
```

##### getAllLinksToNodes(object, tool)
- `object` _(String)_ Name of Object
- `tool` _(String)_ Name of Tool
- **Returns** _(Object)_ returns an object with all links stored with a single tool

Return all nodes attached to a tool. 

```javascript
var allLinksStoredWithTool = server.getAllLinksToNodes("feederStation", "scale");
```

##### subscribeToNewFramesAdded(objectName, callback)  // should be tools
- `object` _(String)_ Name of Object
- `callback` _(Function)_
- **Returns** tool _(Object)_ returns reference to the Tool added

It will trigger a callback anytime a new tool gets added to Object. 

```javascript
server.subscribeToNewFramesAdded("feederStation", function(tool){
    var newtool = tool;
});
```

##### subscribeToReset(objectName, callback)
- `object` _(String)_ Name of Object
- `callback` _(Function)_

The callback is triggered when the system is reset, or Git is resetting the system.

```javascript
server.subscribeToReset("feederStation", function(){
    // called when system is reset
});
```


##### subscribeToUDPMessages(callback)
- `callback` _(String)_
- **Returns** _(Object)_ UDP messages

Subscribe to UDP messages. You can use this for listening to action messages that are used to keep the overall system updated. 


```javascript
server.subscribeToNewFramesAdded("feederStation", function(udpMsg){
    var aMessage = udpMsg;
});
```

##### pushUpdatesToDevices(object)
- `object` _(String)_ Name of Object

Force all Spatial Toolbox Applications to reload this Object.

```javascript
server.pushUpdatesToDevices("feederStation");
```

##### activate(object)
- `object` _(String)_ Name of Object

Activate an Object. It will be visible to the Spatial Toolbox. By default, all new Objects are active.

```javascript
server.activate("feederStation");
```

##### deactivate(object)
- `object` _(String)_ Name of Object

Deactivate an Object. It will be visible to the Spatial Toolbox. By default, all new Objects are active.

```javascript
server.deactivate("feederStation");
```

##### getObjectIdFromObjectName(object)
- `object` _(String)_ Name of Object
- **Returns** _(Object)_ Object UUID

Get UUID of Object via Name

```javascript
var ObjectUuid = server.getObjectIdFromObjectName("feederStation");
```

<!--- ##### getMarkerSize
objectName return getMarkerSize

```javascript
server.
```


```javascript
server.
```
--->

##### addEventListener(option, callBack)
- `option` _(String)_ Options for System. Use any of these strings `reset`, `shutdown`, `initialize`
- `callback` _(Function)_

Callback for all kinds of system events. Currently supported: `reset`, `shutdown`, `initialize`.

```javascript
server.addEventListener("reset", function(){
    // place code here for when the system executes a reset
});
```

<!---
##### advertiseConnection
(describe this later on.) Cool functionality but maybe overwelming for now

```javascript
server.
```
--->
