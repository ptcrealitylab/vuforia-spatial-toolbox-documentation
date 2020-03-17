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
One way to define when a Tool becomes moveable is the moveDelay described above. Another way is to define specific areas that are interactive elements and as a consequence touching all other areas makes the tool instantly movable. Call this function to envoke this functionality. Asign all your interactive elements the class "spatial interaction" to make them interactive elements.

```javascript
spatialTool.enableCustomInteractionMode();
```


You call this function to specify what becomes interactable and what moves the tool
Touches emediatly move frame around
unless you touch on a div that has a specific class called "spatial interaction"

##### enableCustomInteractionModeInverted
If you want nothing but one thing to move the frame around

```javascript
spatialTool.
```

##### setInteractableDivs
once the one before is set, you can give this the IDs of the divs that are interactable.

(divList [array of actual div])

```javascript
spatialTool.
```

##### subscribeToFrameCreatedEvents

callback anytime the user creates a new frame while this frame is visible
frameID, frame type inside object

```javascript
spatialTool.
```

##### subscribeToFrameDeletedEvents
same callback but if deleted

```javascript
spatialTool.
```

##### ignoreAllTouches

Tells Tools to do pointer events none you can not do anything with it.
(true/false)

```javascript
spatialTool.
```

##### registerTouchDecider

For threejs to know if you tap something that should count as touched. 

You hand over a function whos job is to return true or falls in case something is hit or not.

(hand over function) In fullscreen frames

```javascript
spatialTool.
```

##### unregisterTouchDecider
 cancel the touch decider. switch between fullscreen and not full screen
 ()
 
```javascript
spatialTool.
```

##### changeFrameSize

Updating the tools knowlage of the size of an object
(with, height)

```javascript
spatialTool.
```

##### getScreenDimensions
get screen dimensions of the actual device. threejs needs it to set aspect

callback (with, heigh)

<!---
##### addInterfaceListener
What menu button do I push right now. 
callback (menu button string)

##### getInterface         
returns what interface is activated
--->
      


### Spatial Edge Server Communication 
The previous function handle the communication and behavour within the Spatial Toolbox. The following functions handle the communication with the Spatial Edge Server that owns the Object in which the Spatial Tool is registered.

##### write
Write flow data to a node owned by the Spatial Tool. This flow data is of a simple floating data type with values between 0.0 and 1.0.
The example below, writes 0.5 to the node "nodeName", unit =kg, min = 0kg, max = 10kg and the last entry tells that the value is only updated on change. All values exept nodeName and the Value are optional.

```javascript
spatialTool.write("nodeName", 0.5, "f", "kg", 0, 10, false);
```

##### getUnitValue

Returns the real Value and Unit of a node flow data package. The Value is mapped to the minimum and maximum. For example a data package with a value of 0.5, a min = 0, max = 10 and unit = "kg" will return a alue of 5 and the unit "kg".

```javascript
valueAndUnitObject = spatialTool.getUnitValue(flowDataPackage)
```

##### spatial.addReadListener
Add a readlistzener to read value changes from a node. These changes are synchronized with the Edge Server for as long as the Spatial Tool active. The tool becomes inactive 3 seconds after its not visible anymore. 

```javascript
spatialTool.addReadListener("nodeName", function(e){
	var package = spatialTool.getUnitValue(e);
	value = package.value;
	unit = package.unit;
});
```

##### readRequest
This forces the server to send the latest value, so that the readListener can pick it up. You can use this in combination with addReadListener to initialize your spatial Tool if needed.

```javascript
spatialTool.readRequest("nodeName");

spatialTool.addReadListener("nodeName", function(e){
	var package = spatialTool.getUnitValue(e);
	value = package.value;
	unit = package.unit;
});
```

()

##### writePublicData 
PublicData is a json object that contains data objects specified by the server side node definition. You need to finetune your Tool via specific node types. publicData allows you to handle complex data among the server side node programm and thr Spatial Tool. The value can contain any JSON encoded data. The last parameter is optional and if set to true, forces the server not to store the data but handle it as realtime flow.

```javascript
spatialTool. writePublicData("nodeName", "key", value, false);
```

##### writePrivateData
Unlike publicData, privateData is write only and its content is only read accessable to the edge server. Spatial Tools and the Spatial Toolbox will never have read access.

```javascript
spatialTool.writePublicData("nodeName", "key", value);
```


##### readPublicData 
Read data from publicData once. Use this function if you need to fill your spatial Tool with all current publicData values. The last parameter is optional and defines a default value in case the key does not jet exist in the publicData.

```javascript
value = spatialTool.readPublicData("nodeName", "key", value);
```

##### addReadPublicDataListener 
Add a a readlistener for publicData to read value changes for a defined Key. These changes are synchronized with the Edge Server for as long as the Spatial Tool active. The tool becomes inactive 3 seconds after its not visible anymore. 

```javascript
spatialTool.addReadPublicDataListener("nodeName", "key", function(e){
var value = e;
});
```

##### reloadPublicData 

Sends a message to the edge server to send the most recent state of publicData. You can use this to initialize your spatial Tool, since the publicDataListener only reacts to updates. 

```javascript
value = spatialTool.reloadPublicData();

spatialTool.addReadPublicDataListener("nodeName", "key", function(e){
	var value = e;
});
```

## Create new Interfaces

### API Reference


##### write
write data to a specific object frame node

objectName, frameName, nodeName, value, mode, unit, min, max

```javascript
spatialTool.
```

##### writePublicData
objectName, frameName, nodeName, dataObject (value name), data

```javascript
spatialTool.
```

##### clearObject
Older APIS remove IO points which are no longer needed. 
Deletes all nodes 

ObjectID, Frame ID

```javascript
spatialTool.
```

##### removeAllNodes
Remove all nodes via Name and frame name 

```javascript
spatialTool.
```

##### reloadNodeUI
reload this object within all open editors... important to refresh the tool in the toolbox
(objectName)

```javascript
spatialTool.
```


##### getAllObjects
returns all objects object

```javascript
spatialTool.
```

##### getKnownObjects
get all known object among the entire network
object of objects IDs 

{uuid {version, protocol, ip}}

```javascript
spatialTool.
```

##### getAllFrames
(objectName)
return all frames that this object has 

```javascript
spatialTool.
```

##### getAllNodes
{objectName, frameName)

just the nodes

```javascript
spatialTool.
```

##### getAllLinksToNodes
objectname, framename
returns all links known to frame

```javascript
spatialTool.
```

##### subscribeToNewFramesAdded
object name, callback 
it will triger tallback anytime frame gets added to object. 
callback(frame)

```javascript
spatialTool.
```

##### subscribeToReset
Screen hardware interface 
if git reset commit is pushed this can help to reinit

callback();

```javascript
spatialTool.
```


##### subscribeToUDPMessages

you can listen to action events trough this

msgContent 

action messages for example

```javascript
spatialTool.
```


##### addNode
adds a node

objectNmae, frameName, NodeName, type, possition (){x,y}

```javascript
spatialTool.
```


##### renameNode
objectNmae, frameName, old nodeName, newNodeName

```javascript
spatialTool.
```

##### moveNode
objectNmae, frameName, NodeName, x,y,scale, matrix, loyalty (optional)

```javascript
spatialTool.
```

##### removeNode
objectNmae, frameName, NodeName,

```javascript
spatialTool.
```

##### attachNodeToGroundPlane
objectNmae, frameName, NodeName, boolean

```javascript
spatialTool.
```

##### pushUpdatesToDevices
objectName forces the toolboxes to reload

```javascript
spatialTool.
```

##### activate
activates an object
()

```javascript
spatialTool.
```

##### deactivate
deactivates an object
()

```javascript
spatialTool.
```

##### getObjectIdFromObjectName
Get UUID of Object via Name

Name return UUID

```javascript
spatialTool.
```

##### getMarkerSize
objectName return getMarkerSize

```javascript
spatialTool.
```

##### addReadListener
objectNmae, frameName, NodeName, callback (dataObject)

```javascript
spatialTool.
```

##### addPublicDataListener
objectNmae, frameName, NodeName, dataObject(valueListen for), callback (publicData)
what ever public data is

```javascript
spatialTool.
```

##### addConnectionListener
objectNmae, frameName, NodeName, callback (whole link structure constructor)
 
 subscribe if link is created
 
```javascript
spatialTool.
```

##### removeReadListeners
objectNmae, frameName, NodeName,
() remove a read listner 

```javascript
spatialTool.
```

##### map
value, in min, in max, out min, out max

```javascript
spatialTool.
```

##### addEventListener
reset and shutdown

(option string (callback))

Closing serial ports and stuff to 

```javascript
spatialTool.
```

##### advertiseConnection
(describe this later on.) Cool functionality but maybe overwelming for now

```javascript
spatialTool.
```

##### loadHardwareInterface
(_dirName)
HardwareInterfaceName
settings.json file and return json content 

```javascript
spatialTool.
```

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


