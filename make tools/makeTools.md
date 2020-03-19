# Introduction

<!---<img align="right" width="150" height="400" src="folder.svg">--->

To build new tools, you need to know a view basics:

1. All data is stored, processed and handled via nodes.
2. Nodes are little programms that live in the Spatial Edge Server. Some are visible to the user in physical space, some are hidden and only serve as data endpoints. Nodes can represent the physical functionality of a real world object, or just a virtual functionality of a spatial tool.
3. No matter what functionality a node has, it always belong to a Tool and Tools are attached to objects in space.
4. Node templates are stored on the server and instantiated into your tool everytime a new Tool is placed into the world.

The following APIs will help you to define how a spatial tool behaves, how it connects to the Edge Server and how it instantiates the nodes.

<!--- <br clear="right"/> --->

# Create new Tools
What is a spatial tool? It is like any other web application just with a spatial context component to it. This means, a plain HTML web application is located hovering in space and a webGL application uses a full screen web frame that can use any webGL framework to possition content in physical space.

How to get started? Create the following folderstructure within the vuforia-spatial-edge-server folder: ```/addons/<yourAddon>/tools/<newToolName>```. Place your spatial tool into this folder. Startpage is index.html

The following APIs will help you to configure your spatial tool, allow the right visualisation within the Spatial Toolbox, connect with other tools and use the logic capabilities of the Spatial Toolbox. 


 Your tools will be available to all objects that are hostet from the server they are stored on. This means, if one of your objects is visible in the Spatial Toolbox, the Spatial Toolbox can make use of them.


## API Reference


### Initialize the Javacript Library within your Spatial Tool
Include the Spatial Toolbox functionality by instancing as following in your Java Script code:

```javascript
var spatialTool = new SpatialTool();
```

### Communication with other Spatial Tools
The Vuforia Toolbox allows each tool to communicate with other tools that are curently visible in the Spatial Toolbox.


##### sendGlobalMessage(message)
- `message` _(String)_ message to be sent

Send messages that broadcast to all other spatial tools currently visible in the Spatial Toolbox.

```js
spatialTool.sendGlobalMessage("Hello World");
```

##### sendMessageToFrame(toolUUID, message)
- `toolUUID ` _(String)_ Unique ID for a spatial tool
-  `message` _(String)_ Message to be sent

Message to a specific tool via frameKey

```js
spatialTool.sendGlobalMessage("destinationToolUUID", "Hello World");
```

<!---
##### sendCreateNode
Create a new node that belongs to the spatial tool.

```javascript
spatialTool.sendCreateNode("nodeName", x, y, attachTogroundplane, noDoublicate);
```

##### initNode
Create a node on that frame

```javascript
spatialTool. initNode(name, type, x, y, scale, default);
```

##### sendMoveNode
Move a node around
name, x,y

##### sendResetNodes
Removes all nodes from frame
()
--->

##### addGlobalMessageListener(callback)
- `callback ` _(Function)_
- **Returns** `message ` _(String)_ Message recived

Allows a spatial tool to listen to messages broadcasted by other spatial tools.

```javascript
spatialTool.addGlobalMessageListener(function(message){
  console.log(message);
});
```
##### addFrameMessageListener(callback)
- `callback ` _(Function)_
- **Returns** `message ` _(String)_ Message recived

Only listen to messages that are addessed to this specific spatial tool.

```javascript
spatialTool.addFrameMessageListener(function(message){
  console.log(message);
});
```

### Subscribe to Matrices
The Spatial Toolbox allows every spatial tool to subscribe to a varaiaty of matrices. These matrices can be used for example to calculate the distance to other spatial tools or they allow the use of webGL for spatial tools that require 3D content.


##### subscribeToMatrix()
Subscribe to the modelView and projectionMatrix. These matrices locate the spatial origin point for your tool relative to the object the tool is attached to. This can be an object or world object.

```javascript
spatialTool.subscribeToMatrix();
```

##### subscribeToScreenPosition()
Subscribe to the 2D screen position for a 3D spatial tool position in space. 

```javascript
spatialTool.subscribeToScreenPosition();
```

##### subscribeToDevicePoseMatrix()
Subscribe to the devicePoseMatrix. This matrix describes where the device is located in space relative to the world object origin.

```javascript
spatialTool.subscribeToDevicePoseMatrix();
```


##### subscribeToAllMatrices()
Subscribe to the modelView matrices for all visible tools.

```javascript
spatialTool.subscribeToAllMatrices();
```

##### subscribeToGroundPlaneMatrix()
Subscribe to the groundplane Matrix. This matrix will match the world object origin once the world object is seen by the device.

```javascript
spatialTool.subscribeToGroundPlaneMatrix();
```

##### addMatrixListener(callback)
- `callback ` _(Function)_ This callback is syncronized with video frame rate.
- **Returns** `modelview` _(Number[16])_ ModelViewMatrix 
- **Returns** `projection ` _(Number[16])_ ProjectionMatrix

Listen to modelView Matrix updates for your single spatial Tool. It only works once subscribeToMatrix() is called. This listener is synchronized with the video update rate. Use the callback of this listener to synchronize animations.

```javascript
spatialTool.addMatrixListener(function(modelview, projection){
  var modelViewMatrix = modelview;
  var projectionMatrix = projection;
});
```

##### addAllObjectMatricesListener(callback)
- `callback` _(Function)_ This callback is syncronized with video frame rate.
- **Returns** `allModel` _(Object)_ ModelViewMatrices for all visible Tools with the format _toolUUID (String): modelViewMatrix (Number[16])_.
- **Returns** `projection ` _(Number[16])_ ProjectionMatrix

Listen to modelView Matrix updates for ALL visible spatial tools. It only works once subscribeToAllMatrices() is called. This listener is synchronized with the video update rate. You can use the callback of this listener to synchronize animations.

```javascript
spatialTool.addAllObjectMatricesListener(function(allModel,preojection){
 var allModelviewMatrices = allModel; // object of matrices arrays
 var projectionMatrix = preojection;
});
```

##### addDevicePoseMatrixListener(callback)
- `callback ` _(Function)_ This callback is syncronized with video frame rate.
- **Returns** `devicePose ` _(Number[16])_ DevicePoseMatrix 
- **Returns** `projection ` _(Number[16])_ ProjectionMatrix

Listen to device pose Matrix updates from your device. It only works once the related subscription is called. This listener is synchronized with the video update rate.

```javascript
spatialTool.addDevicePoseMatrixListener(function(devicePose, projection){
  var devicePoseMatrix = devicePose;
  var projectionMatrix = projection;
});
```

##### addScreenPositionListener(callback)
- `callback ` _(Function)_ This callback is syncronized with video frame rate.
- **Returns** `devicePose ` _(Object)_ Object with x _(Number)_ and y _(Number)_ coordinates

Listen to the 2D screen position for any spatial tool. It only works once the related subscription is called. This listener is synchronized with the video update rate.

```javascript
spatialTool.addScreenPositionListener(function(position){
	var x = position.x;
	var y = position.y;
});
```

##### cancelScreenPositionListener()
Remove all matrix listeners.

```javascript
spatialTool. cancelScreenPositionListener();
});
```

##### getPositionX(), getPositionY(), getPositionZ()
- **Returns** _(Number)_ x,y or z

Returns a number for translation distance and position between the spatial tool and the device.

```javascript
x =  spatialTool.getPositionX();
y =  spatialTool.getPositionY();
z =  spatialTool.getPositionZ();
```

##### getProjectionMatrix()
- **Returns** _(Number[16])_ projection matrix 

Returns the current projection Matrix in the format m[16]

```javascript
projectionMatrix =  spatialTool.getProjectionMatrix();
```

##### getModelViewMatrix()
- **Returns** _(Number[16])_ model-view matrix

Returns the current modelView Matrix in the format m[16]

```javascript
modelViewMatrix =  spatialTool.getModelViewMatrix();
```

##### getGroundPlaneMatrix()
- **Returns** _(Number[16])_ ground-plane matrix 

Returns the current groundPlane Matrix in the format m[16]

```javascript
groundPlane =  spatialTool.getGroundPlaneMatrix();
```

##### getDevicePoseMatrix()
- **Returns** _(Number[16])_ device-pose matrix

Returns the current device pose Matrix in the format m[16]

```javascript
devicePose =  spatialTool.getDevicePoseMatrix();
```

##### getAllObjectMatrices()
- **Returns** _(Object)_ All matrices

Returns all current modelView Matrices in the format `{"objectUuid": number[16], ...}`

```javascript
allMatrices =  spatialTool. getAllObjectMatrices();
```

### Control AR Screen Position
Spatial Tools can exist of a simple HTML page or webGL content. Some tools require fullscreen modes or persist in space even the related object is not visible. WebGL content always requite a fullscreen mode since the webgl context takes over the spatial transformations.

##### setFullScreenOn()
Set your tool to fullscreen mode. Use this non spatial 2D UIs or any WebGL context.

```javascript
spatialTool.setFullScreenOn();
```
##### setFullScreenOff()
Switches the full screen mode off and attaches the tool back to its spatial position.

```javascript
spatialTool.setFullScreenOff();
```
##### setStickyFullScreenOn()
Set your tool to permanent fullscreen mode. Use this mode for 2D UI screens, so that the UI does not dissapear when the attached object is out of view. 

```javascript
spatialTool.setStickyFullScreenOn();
```

##### setStickinessOff()
Remove the stickyness from the Fullscreen mode.

```javascript
spatialTool.setStickinessOff();
```

##### setExclusiveFullScreenOn()
Set your tool to permanent fullscreen mode. This mode will remove any other fullscreen mode that is called with the same exclusivity. The callback is called when the fullscreen mode is killed.

```javascript
spatialTool.setExclusiveFullScreenOn(callback);
```

##### setExclusiveFullScreenOff()
Switches the exclusive full screen mode off and attaches the tool back to its spatial position.

```javascript
spatialTool.setExclusiveFullScreenOff();
```

### Media Content
These functions help to record videos, images or use the device buildin keyboard.

##### startVideoRecording()
The Spatial Toolbox will start recording the screen in the background. The final video will automatically be saved once the video is stoped and attached to your spatial tool.

```javascript
spatialTool.startVideoRecording();
```

##### stopVideoRecording(callback)
- `callback ` _(Function)_ The callback provides the video URL
- **Returns** url _(String)_ The url for the video

Stop the video recording and attach it to your tool.

```javascript
spatialTool.startVideoRecording(function(url){
	var videoURL = e;
});
```

##### announceVideoPlay()
On certain Mobile Devices the playback of multiple videos in space can be difficult. Use this function to anounce that a single video is played. 

```javascript
spatialTool.announceVideoPlay();
```

##### subscribeToVideoPauseEvents(callback)
- `callback ` _(Function)_

Use this event to programm your video player to pause when other videos are playing. In a best case, try to unload your video and replace it with a screenshot to save resources for other Spatial Tools.

```javascript
spatialTool.subscribeToVideoPauseEvents(function(){
	// called when pause is requested.
});
```

##### getScreenshotBase64(callback)
- `callback ` _(Function)_
- **Returns** image _(String)_ Image encoded in base64 string. 

Screenshot of the Vuforia Camera. A single frame returned as a base64 string.

```javascript
spatialTool.getScreenshotBase64(function(image){
	var imageBase64String = image;
});
```

##### openKeyboard()
Open the Keyboard

```javascript
spatialTool.openKeyboard();
```

##### closeKeyboard()
Close the Keyboard

```javascript
spatialTool.closeKeyboard();
```

##### onKeyboardClosed(callback)
- `callback ` _(Function)_

Callback if keyboard is closed

```javascript
spatialTool.onKeyboardClosed(function(){
	// called on keyboard closed
});
```

##### onKeyUp(callback)
- `callback ` _(Function)_
- **Returns** key _(Object)_ Keybord event

Use this to listen to keyboard entries. The callback returns the entire keyboard event.


```javascript
spatialTool.onKeyUp(function(key){
	var keyboardEvent = key;
});
```

### Tool Behavour 


##### setVisibilityDistance(distance)
- `distance ` _(Number)_ number in meter to define distance

Define the distance (in meter) your spatial Tool is visible in the Spatial Toolbox. A user can change this number via the Spatial Toolbox UI.

```javascript
spatialTool.setVisibilityDistance(2);
```

##### addVisibilityListener(callback)
- `callback ` _(Function)_
- **Returns** key _(Object)_ Keybord event

Callback to read the visibility of a spatial tool. The interface stays active for 3 seconds after it becomes invisible.

```javascript
spatialTool.addVisibilityListener(function(visible){
  var isVisible = visible;
});
```

##### getVisibility()
- **Returns** _(Boolean)_

Returns true or false if the spatial tool is visible.

```javascript
spatialTool.getVisibility();
```

##### setMoveDelay(delay)
- `delay ` _(Number)_ number in milliseconds

Set how long it takes (in ms) for a user to tap and hold a spatial tool until it becomes moveable. 

```javascript
spatialTool.setMoveDelay(10);
```

##### addIsMovingListener(callback)
- `callback ` _(Function)_
- **Returns** move _(Boolean)_

Returns true if a tool is moved and false once it is released.

```javascript
spatialTool.addIsMovingListener(function(move){
  var isMoving = move;
});
```


##### enableCustomInteractionMode()
One way to define when a Tool becomes moveable is the moveDelay described above. Another way is to define specific areas that are interactive elements and as a consequence touching all other areas makes the tool instantly movable. Call this function to envoke this functionality. Asign all your interactive elements the class `spatial interaction` to make them interactive elements.

```javascript
spatialTool.enableCustomInteractionMode();
```

##### enableCustomInteractionModeInverted()
This does the invert to the function above. In this case the elements defined by the next function become the item that allows the spatial moving.

```javascript
spatialTool.enableCustomInteractionModeInverted()
```

##### setInteractableDivs(divList)
- `divList ` _(String[])_ Array of interactive html elements

Once `enableCustomInteractionMode`or `enableCustomInteractionModeInverted`use this function to define the interactive elements.



```javascript
  realityInterface.enableCustomInteractionMode();
  realityInterface.setInteractableDivs([touchInteractiveElement]);
```

##### subscribeToFrameCreatedEvents(callback)
- `callback ` _(Function)_
- **Returns** frameUuid _(String)_ The Uuid of the frame created
- **Returns** type _(String)_ the type of the frame (e.g. graph, slider, etc)


Triggers a callback anytime another new frame is created while this frame is loaded in the DOM.

```javascript
spatialTool.subscribeToFrameCreatedEvents(function(frameUuid, type){
var thisFrameIsNew = frameUuid;
var itsTypeIs = type;
});
```

##### subscribeToFrameDeletedEvents
- `callback ` _(Function)_
- **Returns** frameUuid _(String)_ The Uuid of the frame deleted
- **Returns** type _(String)_ the type of the frame (e.g. graph, slider, etc)

Triggers a callback anytime a frame is deleted while this frame is loaded in the DOM.

```javascript
spatialTool.subscribeToFrameDeletedEvents(function(frameUuid, type){
var thisFrameIsDeleted= frameUuid;
var itsTypeWas = type;
});
```

##### ignoreAllTouches(newValue)
- `newValue` _(Boolean)_

Pass in true (or omit the argument) to make the tool set pointer-events none so all touches pass through un-altered

```javascript
spatialTool.ignoreAllTouches(false);
```

##### registerTouchDecider(callback)
- `callback ` _(Function)_ A callback function that decides if our tool is touched or not. 
- **Returns** eventData _(Object)_ touch event includes x and y of screen touch.
- **Returns** _(Boolean)_ Your callback should return a boolean to decide if a touch is inside an element or outside.

Use this function in a fullscreen tool for when you use for example uses threejs or similar. You need to pass a function that decides if a touch event is consumed with your experiance.

```javascript
spatialTool.registerTouchDecider(function(eventData){
  return doSomethingThatDecides(eventData.x, eventData.y);
});
```

##### unregisterTouchDecider()
 Cancel the touch decider callback.
 
```javascript
spatialTool.unregisterTouchDecider();
```

##### changeFrameSize(newWidth, newHeight)
- `newWidth` _(Number)_ A new touch overlay width
- `newHeight ` _(Number)_ A new touch overlay height

Adjust the size of the tool's touch overlay element to match the current size of this frame.

```javascript
spatialTool.changeFrameSize(640, 480);
```

##### getScreenDimensions(callback)
- `callback ` _(Function)_
- **Returns** _(Number)_ screen dimension width
- **Returns** _(Number)_ screen dimension height

Asynchronously query the screen width and height from the parent application, as the iframe itself can't access that.

```javascript
spatialTool.registerTouchDecider(function(width, height){
	screenDimensionWidth = width;
	screenDimensionHeight = height;
});
```

<!---
##### addInterfaceListener
What menu button do I push right now. 
callback (menu button string)

##### getInterface         
returns what interface is activated
--->
      


### Spatial Edge Server Communication 
The previous function handle the communication and behavour within the Spatial Toolbox. The following functions handle the communication with the Spatial Edge Server that owns the Object in which the Spatial Tool is registered.

##### write(node, value, mode, unit, unitMin, unitMax, forceWrite)
- `node ` _(String)_ Name of node
- `value ` _(Number)_ The new value for the node. The range <u>must</u> be between 0.0 and 1.0.
- `mode ` _(String)_ [optional] Data mode. Currently `f` for float is the only supported choice.
- `unit ` _(String)_ [optional] The unit of your data such as 'kg', 'minutes', 'liter', .. 
- `unitMin ` _(String)_ [optional] value range minimum
- `unitMax ` _(String)_ [optional] dara range maximum
- `forceWrite ` _(Boolean)_ [optional] Sends the value even if it hasn't changed since the last write.


Write flow data to a node owned by the Spatial Tool. This flow data is of a simple floating data type with values between 0.0 and 1.0.
The example below, writes 0.5 to the `node` = "nodeName", `unit` = kg, `min` = 0, `max` = 10 and the last entry tells that the value is only updated on change. The resulting value will be 5 kg. 

```javascript
spatialTool.write("nodeName", 0.5, "f", "kg", 0, 10, false);
```

##### getUnitValue(flowDataObject)
- `flowDataObject ` _(String)_ flow data object
- **Returns** _(Object)_ real value (mapped 0 - 1 to min - max) and unit

Returns the real Value and Unit of a node flow data object. The Value is mapped to the minimum and maximum. For example a data package with a value of 0.5, a min = 0, max = 10 and unit = "kg" will return a alue of 5 and the unit "kg".

```javascript
valueAndUnitObject = spatialTool.getUnitValue(flowDataObject)
```

##### spatial.addReadListener(node, callback)
- `node ` _(String)_ name of Node
- - `callback ` _(Function)_ is called anytime the server provides a new data object for the node.
- **Returns** flowDataObject _(Object)_ flow data object for the node

Add a readlistener to read value changes from a node. These changes are synchronized with the Edge Server for as long as the Spatial Tool active. The tool becomes inactive 3 seconds after its not visible anymore. 

```javascript
spatialTool.addReadListener("nodeName", function(flowDataObject){
	var realValues = spatialTool.getUnitValue(flowDataObject);
	value = realValues.value;
	unit = realValues.unit;
});
```

##### readRequest(node)
- `node ` _(String)_ name of Node

This forces the server to send the latest value, so that the readListener can pick it up. You can use this in combination with addReadListener to initialize your spatial Tool if needed.

```javascript
spatialTool.readRequest("nodeName");

spatialTool.addReadListener("nodeName", function(e){
	var package = spatialTool.getUnitValue(e);
	value = package.value;
	unit = package.unit;
});
```


##### writePublicData(node, valueName, value, realtimeOnly) 
- `node ` _(String)_ name of Node
- `valueName ` _(String)_ publicData is defined by the node it self. This is a key to select an object within public Data.
- `value ` _(Object/String/Number)_ Data you want to store with key.
- `realtimeOnly ` _(Boolean)_ doesn't send a post message to reload object, allows rapid stream.


PublicData is a json object that contains data objects specified by the server side node definition. You need to finetune your Tool via specific node types. publicData allows you to handle complex data among the server side node programm and thr Spatial Tool. The value can contain any JSON encoded data. The last parameter is optional and if set to true, forces the server not to store the data but handle it as realtime flow.

```javascript
spatialTool. writePublicData("nodeName", "key", anyValue, false);
```

##### writePrivateData(node, valueName, value)
- `node ` _(String)_ name of Node
- `valueName ` _(String)_ writePrivateData is defined by the node it self. This is a key to select an object within private Data.
- `value ` _(Object/String/Number)_ Data you want to store with key.

Unlike publicData, privateData is write only and its content is only read accessable to the edge server. Spatial Tools and the Spatial Toolbox will never have read access.

```javascript
spatialTool.writePublicData("nodeName", "key", anyValue);
```

##### readPublicData(node, valueName, value)
- `node ` _(String)_ name of Node
- `valueName ` _(String)_ This is a key to select an object within publicData.
- `value ` _(Number)_ [Optional] If value is undefined this default value is used.
- **Returns** _(Object/String/Number)_ returns the stored data.

Read data from publicData once. Use this function if you need to fill your spatial Tool with all current publicData values. The last parameter is optional and defines a default value in case the key does not jet exist in the publicData.

```javascript
value = spatialTool.readPublicData("nodeName", "key", value);
```

##### addReadPublicDataListener(node, valueName, callback)
- `node ` _(String)_ name of Node
- `valueName ` _(String)_ This is a key to select an object within publicData.
- `callback ` _(Function)_
- **Returns** value _(Object/String/Number)_ returns the stored data.


Add a a readlistener for publicData to read value changes for a defined Key. These changes are synchronized with the Edge Server for as long as the Spatial Tool active. The tool becomes inactive 3 seconds after its not visible anymore. 

```javascript
spatialTool.addReadPublicDataListener("nodeName", "key", function(value){
var publicDataValue = value;
});
```

##### reloadPublicData()

Sends a message to the edge server to send the most recent state of publicData. You can use this to initialize your spatial Tool, since the publicDataListener only reacts to updates. 

```javascript
spatialTool.reloadPublicData();
spatialTool.addReadPublicDataListener("nodeName", "key", function(value){
	var publicDataValue = value;
});
```

## Create new Interfaces

Interfaces are plugins that allows to connect any hardware or build out any service and connect it with the Spatial Toolbox. The Spatial Toolbox processes data via spatial nodes. They are the central data storing and data processing element within the Spatial Toolbox. Therefore most of the API in this chaptor focuses how to handle nodes and therefore how to connect your interface to the server.

#### Folder Structure
Your interface-addon should be stored in the addons/interfaces/[yourInterfaceAddon]/ folder and it requires the following file file:

- `index.js` this file defines the entry point for your spatial edge server interface.


### API Reference

##### Initialize the interface

```js
//Enable this hardware interface
var server = require('@libraries/hardwareInterfaces');
var settings = server.loadHardwareInterface(__dirname);

exports.enabled = settings('enabled');
exports.configurable = true; // can be turned on/off/adjusted from the web frontend

if (exports.enabled) {

// plase your interface code here

}

```

##### loadHardwareInterface(`_dirName`)
- `_dirName` _(String)_ Must use string `_dirName` and returns the path to executed file
- **Returns** _(Object)_ Returns content of the settings json file stored for this interface

Returns access to the stored settings for this Inteface

```javascript
var settings = server.loadHardwareInterface(__dirname);
```

##### addNode(object, tool, node, type, position) 
- `object` _(String)_ Name of Object
- `tool` _(String)_ Name of Tool
- `node ` _(String)_ Name of node
- `type ` _(String)_ Name of the node Type. The server need to have a node of this type otherwise it will use a default type.
- `position ` _(Object)_ [Optional] {x: float, y: float} object for the node's starting position. otherwise random.

Adds a new node to a Tool. If the Tool does not exist, it automatically generates a default tool with the `tool`-name. If the Object does not exist, it automatically generates a default object with the `object`-name. This function should be the first you call to initialize all required nodes.

```javascript
server.addNode("feederStation", "scale", "scaleValue", "node", {x: 1, y: 1})
```


##### renameNode(object, tool, oldNode, newNode)
- `object` _(String)_ Name of Object
- `tool` _(String)_ Name of Tool
- `oldNode ` _(String)_ Name of existing node
- `newNode ` _(String)_ new name for the existing node

Rename an existing node


```javascript
server.renameNode("feederStation", "scale", "scaleValue", "measurement");
```

##### moveNode(object, tool, node, x, y, scale, matrix, loyalty) 
- `object` _(String)_ Name of Object
- `tool` _(String)_ Name of Tool
- `node ` _(String)_ Name of node
- `x` _(String)_ new x coordinate relative to the node matrix transform
- `y` _(String)_ new y coordinate relative to the node matrix transform
- `scale` _(String)_ new scale relative to the node matrix transform
- `matrix` _(Number[16])_ new matrix transform
- `loyalty` _(String)_ define loyalty of object (grounPlane, object, world, ...)


objectNmae, frameName, NodeName, x,y,scale, matrix, loyalty (optional)

```javascript
server.moveNode("feederStation", "scale", "measurement", 1, 1, 1, identityMatrix, "object"); 
```

##### removeNode(object, tool, node)
- `object` _(String)_ Name of Object
- `tool` _(String)_ Name of Tool
- `node ` _(String)_ Name of node

```javascript
server.removeNode("feederStation", "scale", "measurement");
```

##### attachNodeToGroundPlane(object, frame, node, shouldAttachToGroundPlane)
- `object` _(String)_ Name of Object
- `tool` _(String)_ Name of Tool
- `node ` _(String)_ Name of node
- `shouldAttachToGroundPlane ` _(Boolean)_ Define if node is attached to the grounPlane

Lets you attach your node to the ground plane. This is similar to loyalty and eventually an API around loyalty will replace this functionality.

```javascript
server.attachNodeToGroundPlane("feederStation", "scale", "measurement", true)
```


##### clearObject(object, tool)
- `object` _(String)_ Name of Object
- `tool` _(String)_ Name of Tool

In case your object handles a lot of changes to the nodes, use this function to clear the memory of unused nodes. You must call this function after you have initialized all your nodes, otherwise you lose data.


```javascript
server.clearObject("feederStation", "scale");
```

##### removeAllNodes(objectName, tool)
- `object` _(String)_ Name of Object
- `tool` _(String)_ Name of Tool

Remove all nodes from your tool

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
- `node ` _(String)_ Name of node
- `value ` _(Number)_ The new value for the node. The range <u>must</u> be between 0.0 and 1.0.
- `mode ` _(String)_ [optional] Data mode. Currently `f` for float is the only supported choice.
- `unit ` _(String)_ [optional] The unit of your data such as 'kg', 'minutes', 'liter', .. 
- `unitMin ` _(String)_ [optional] value range minimum
- `unitMax ` _(String)_ [optional] dara range maximum

Write flow data to a node owned by the Spatial Tool within your Object. This flow data is of a simple floating data type with values between 0.0 and 1.0. The example below, writes 0.5 to the `node` = "nodeName", `unit` = kg, `min` = 0, `max` = 10 and the last entry tells that the value is only updated on change. The resulting value will be 5 kg.
 
```javascript
server.write("feederStation", "scale", "measurement", 0.5, "f", "kg", 0, 10);
```

##### addReadListener(object, frame, node, callBack)
- `object` _(String)_ Name of Object
- `tool` _(String)_ Name of Tool
- `node ` _(String)_ Name of node
- `callback ` _(Function)_ 
- **Returns** flowDataObject _(Object)_ flow data object for the node

Add a readlistener to read value changes from a node.

```javascript
server.addReadListener("feederStation", "scale", "measurement", function(flowDataObject){
	var value = flowDataObject.value;
	var unit = flowDataObject.unit;
});

```

##### removeReadListeners(object, tool)
- `object` _(String)_ Name of Object
- `tool` _(String)_ Name of Tool

Removes all ReadListeners currently running for a tool.

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

Scales an input value to the scope of an output value. Use this function to scale the data flow scale of 0.0 - 1.0 to the scale of your units.

```javascript
var scaleValue = 0.5
var outputValue = server.addReadListener(scaleValue, 0.0, 1.0, 0.0, 10.0);
```

##### writePublicData(object, tool, node, dataObject, data) 
- `object` _(String)_ Name of Object
- `tool` _(String)_ Name of Tool
- `node ` _(String)_ Name of node
- `dataObject` _(String)_ Key to select a dataObject within public Data
- `data ` _(Object/String/Number)_ Data you want to store with the key

PublicData is a json object that contains data objects specified by the node definition. You need to finetune your Tool via specific node types. publicData allows you to handle complex data among the server side node programm and the Spatial Tool. The value can contain any JSON encoded data.

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

Add a a readlistener for publicData to read value changes for a defined Key. These changes are synchronized with the Edge Server and any Spatial Toolbox interacting with the tool.

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

Listen for when there is a new link assosiated with the node.
 
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

Get all known objects among the entire known network

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

##### subscribeToNewFramesAdded(objectName, callback) 
- `object` _(String)_ Name of Object
- `callback` _(Function)_
- **Returns** tool _(Object)_ returns reference to frame added

It will triger a callback anytime a new tool gets added to object. 

```javascript
server.subscribeToNewFramesAdded("feederStation", function(tool){
	var newtool = tool;
});
```

##### subscribeToReset(objectName, callback)
- `object` _(String)_ Name of Object
- `callback` _(Function)_

Callback is triggered when the System is reset or Git is resetting the system.

```javascript
server.subscribeToNewFramesAdded("feederStation", function(){
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

Activate an Object. It will be visible to the Spatial Toolbox. By default all new Objects are active.

```javascript
server.activate("feederStation");
```

##### deactivate(object)
- `object` _(String)_ Name of Object

Deactivate an Object. It will be visible to the Spatial Toolbox. By default all new Objects are active.

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


```javascript
server.
```
<!---
##### advertiseConnection
(describe this later on.) Cool functionality but maybe overwelming for now

```javascript
server.
```--->



## Create new Nodes
The node is the center data storing and data processing element within the Spatial Toolbox. You can define new nodes that your hardware Interface and your spatial tool can use. Each node needs to have a specific structure to function.

#### Folder Structure
Your node-addon should be stored in the addons/nodes/[yourNodeAddon]/ folder and it requires two files as a default:

- `index.js` stores all the logic for your node.
- `gui/index.html` This stores the visible apearance for the node


###index.js

#### generalProperties
- `generalProperties` _(Object)_ defines the data parameters for the new node.
- `name` _(String)_ The name for the Node
- `privateData` _(Object)_ Use this object to define private data. The Spatial Toolbox can only write content to this Object but it can not read content.
- `publicData` _(Object)_ Use this object to define additional data types that your node requires. Keys need to be defined in this object so that the Spatial Toolbox can access them.
- `type` _(String)_ Type should match the folder name.


#### setup(object, frame, node, thisNode)
- `object` _(String)_
- `frame` _(String)_
- `node` _(String)_
- `thisNode` _(Object)_




#### render(object, frame, node, thisNode, callback)
- `object` _(String)_ Currently processed Object UUID
- `frame` _(String)_ Currently processed Frame UUID
- `node` _(String)_ Currently processed Node UUID
- `thisNode` _(Object)_ Reference to the node Object
- `callback ` _(Object)_ callback for when the data flow processing engine should continue.
- **Returns** `object` _(String)_ Currently processed Object UUID
- **Returns** `frame` _(String)_ Currently processed Frame UUID
- **Returns** `node` _(String)_ Currently processed Node UUID
- **Returns** `thisNode` _(Object)_ Reference to the node Object

The Edge Server will call this function on any tick. It will provide the object, frame, node UUIDs as well the entire node Object as refefence. In case you want your node act as an input and output, you must call the callback function, so that an output event is triggered. You can call this callback asynchronous at any time.

All in all, the node logic has 4 parts:

1. generalProperties act as a data storage
2. setup executes a single time
3. render is called by the edge server whenever a new tick triggers trough the logic.
2. spatial data that is saved by the Edge Server and edited via the Spatial Toolbox. This spatial data is accessable via `thisNode` reference.


Example for a delay node:

```javascript
var generalProperties = {
    name: 'delay',
    privateData: {},
    publicData: {delayTime: 1000},
    type: 'delay'
};

exports.properties = generalProperties;

exports.setup = function (_object, _frame, _node, _nodeProperties) {
// add code here that should be executed once.
};

exports.render = function (object, frame, node, thisNode, callback) {
    var delayedValue = thisNode.data.value;

    setTimeout(function() {
        thisNode.processedData.value = delayedValue;
        callback(object, frame, node, thisNode);
    }, thisNode.publicData.delayTime);

};
```

###gui/index.html


## Create new Blocks
The block is similar to a node. However there are significant differences:

1. The Block is an element within a Logic Crafting Board. The Logic Crafting Board is a very simple data flow programming system that allows for spatial programming.
2. As such, blocks have the spatial information of their parent logic Node, however they do not own a spatial component them selves. 
1. The Block expands the simple data flow from the node to four possible in and outputs.
2. The Block requires some additional files within the `gui` folder for it to work within the logic crafting board. 

#### Folder Structure
Your block-addon should be stored in the addons/nodes/[yourBlockAddon]/ folder and it requires the following files:

- `index.js` stores all the logic for your block.
- `gui/index.html` This stores a setup screen that you can use for adjusting the properties for your block stored in `publicData`. 
- `gui/icon.svg` This SVG Image will apear as an icon in the Spatial Toolbox block selection UI. 
- `gui/label.svg` The Label SVG Image will visualize the block within the Logig Crafting Board.

###index.js

#### generalProperties
- `generalProperties` _(Object)_ defines the data parameters for the new node.
- `name` _(String)_ name is displayed underneath icon in block menu
- `blockSize` _(Number)_ Defines the width for your block. It also defines the number of in and outputs.
- `privateData` _(Object)_ Use this object to define private data. The Spatial Toolbox can only write content to this Object but it can not read content.
- `publicData` _(Object)_ Use this object to define additional data types that your node requires. Keys need to be defined in this object so that the Spatial Toolbox can access them.
- `activeInputs` _(Boolean[4])_ Sets which input indices of the block can have links drawn to them
- `activeOutputs`_(Boolean[4])_ Sets which output indices of the block can have links drawn from them
- `nameInput`_(String[4])_ Sets a name for every input
- `nameOutput`_(String[4])_ Sets a name for every output
- `type` _(String)_ Type should match the folder Name.


#### setup(object, frame, node, thisNode)
- `object` _(String)_
- `frame` _(String)_
- `node` _(String)_
- `block` _(String)_
- `thisNode` _(Object)_




#### render(object, frame, node, thisNode, callback)
- `object` _(String)_ Currently processed Object UUID
- `frame` _(String)_ Currently processed Frame UUID
- `node` _(String)_ Currently processed Node UUID
- `block` _(String)_ Currently processed Block UUID
- `thisNode` _(Object)_ Reference to the node Object
- `callback ` _(Object)_ callback for when the data flow processing engine should continue.
- **Returns** `object` _(String)_ Currently processed Object UUID
- **Returns** `frame` _(String)_ Currently processed Frame UUID
- **Returns** `node` _(String)_ Currently processed Node UUID
- **Returns** `block` _(String)_ Currently processed Block UUID
- **Returns** `thisNode` _(Object)_ Reference to the node Object

The Edge Server will call this function similiar to the node on any tick. It will provide the object, frame, node and block UUIDs as well the entire block Object as refefence. In case you want your block act as an input and output, you must call the callback function, so that an output event is triggered. You can call this callback asynchronous at any time.

All in all, the node logic has three parts:

1. generalProperties act as a data storage
2. setup executes a single time
3. render is called by the edge server whenever a new tick triggers trough the logic.


Example for a delay block:

```javascript
var generalProperties = {
    name: 'delay',
    blockSize: 1,
    privateData: {},
    publicData: {delayTime: 1000},
    activeInputs: [true, false, false, false],
    activeOutputs: [true, false, false, false],
    nameInput: ['in', '', '', ''],
    nameOutput: ['out', '', '', ''],
    type: 'delay'
};

exports.properties = generalProperties;

exports.setup = function (object, frame, node, block, thisBlock, callback) {
	// add code here that should be executed once.
};

exports.render = function (object, frame, node, block, index, thisBlock, callback) {
    var delayedValue = thisBlock.data[index].value;

    setTimeout(function() {
        thisBlock.processedData[index].value = delayedValue;
        callback(object, frame, node, block, index, thisBlock);
    }, thisBlock.publicData.delayTime);

};
```

###gui/index.html


###gui/icon.svg


###gui/label.svg


