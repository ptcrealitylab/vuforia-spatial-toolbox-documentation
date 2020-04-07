---
layout: doc
title: API Reference
permalink: /docs/develop/spatial-tools/api-reference
---

<a name="toolApi"></a>
## API Reference

### Index
- [Initialize the Javascript Library within your Spatial Tool](#init)
- [Communication with other Spatial Tools](#comm)
- [Subscribe to Matrices](#subscribe)
- [Control AR Screen Position](#control)
- [Media Content](#media)
- [Tool Behavior](#behavior)
- [Spatial Edge Server Communication](#edgeComm)

<a name="init"></a>
### Initialize the Javascript Library within your Spatial Tool
Include the Spatial Toolbox functionality by instancing as following in your JavaScript code:

```javascript
var spatialInterface = new SpatialInterface();
```
<a name="comm"></a>
### Communication with other Spatial Tools
The Vuforia Toolbox allows each Tool to communicate with other tools that are currently visible in the Spatial Toolbox.


#### sendGlobalMessage(message)
- `message` _(String)_ message to be sent

Send messages that broadcast to all other spatial tools currently visible in the Spatial Toolbox.

```js
spatialInterface.sendGlobalMessage("Hello World");
```

#### sendMessageToFrame(toolUUID, message)
- `toolUUID ` _(String)_ Unique ID for a spatial tool
-  `message` _(String)_ Message to be sent

Message to a specific tool via frameKey

```js
spatialInterface.sendGlobalMessage("destinationToolUUID", "Hello World");
```

<!---
#### sendCreateNode
Create a new node that belongs to the spatial tool.

```javascript
spatialInterface.sendCreateNode("nodeName", x, y, attachTogroundplane, noDoublicate);
```

#### initNode
Create a node on that tool

```javascript
spatialInterface.initNode(name, type, x, y, scale, default);
```

#### sendMoveNode
Move a node around
name, x,y

#### sendResetNodes
Removes all nodes from tool
()
--->

#### addGlobalMessageListener(callback)
- `callback ` _(Function)_
- **Returns** `message ` _(String)_ Message recived

It allows a spatial tool to listen to messages broadcasted by other spatial tools.

```javascript
spatialInterface.addGlobalMessageListener(function(message){
  console.log(message);
});
```
#### addFrameMessageListener(callback)
- `callback ` _(Function)_
- **Returns** `message ` _(String)_ Message recived

Only listen to messages that are addressed to this specific spatial Tool.

```javascript
spatialInterface.addFrameMessageListener(function(message){
  console.log(message);
});
```
<a name="subscribe"></a>
### Subscribe to Matrices
The Spatial Toolbox allows every spatial Tool to subscribe to a variety of matrices. These matrices can be used, for example, to calculate the distance to other spatial tools, or they allow the use of webGL for spatial tools that require 3D content.


#### subscribeToMatrix()
Subscribe to the modelView and projectionMatrix. These matrices locate the spatial origin point for your Tool relative to the Object the Tool is attached to. This can be an object or world object.

```javascript
spatialInterface.subscribeToMatrix();
```

#### subscribeToScreenPosition()
Subscribe to the 2D screen position for a 3D spatial tool position in space. 

```javascript
spatialInterface.subscribeToScreenPosition();
```

#### subscribeToDevicePoseMatrix()
Subscribe to the devicePoseMatrix. This Matrix describes where the device is located in space relative to the world object origin.

```javascript
spatialInterface.subscribeToDevicePoseMatrix();
```


#### subscribeToAllMatrices()
Subscribe to the modelView matrices for all visible tools.

```javascript
spatialInterface.subscribeToAllMatrices();
```

#### subscribeToGroundPlaneMatrix()
Subscribe to the ground-plane Matrix. This Matrix will match the world object origin once the device sees the world object.

```javascript
spatialInterface.subscribeToGroundPlaneMatrix();
```

#### addMatrixListener(callback)
- `callback ` _(Function)_ This callback is syncronized with video frame rate.
- **Returns** `modelview` _(Number[16])_ ModelViewMatrix 
- **Returns** `projection ` _(Number[16])_ ProjectionMatrix

Listen to modelView Matrix updates for your single spatial Tool. It only works once subscribeToMatrix() is called. This listener is synchronized with the video update rate. Use the callback of this listener to synchronize animations.

```javascript
spatialInterface.addMatrixListener(function(modelview, projection){
  var modelViewMatrix = modelview;
  var projectionMatrix = projection;
});
```

#### addAllObjectMatricesListener(callback)
- `callback` _(Function)_ This callback is syncronized with video frame rate.
- **Returns** `allModel` _(Object)_ ModelViewMatrices for all visible Tools with the format _toolUUID (String): modelViewMatrix (Number[16])_.
- **Returns** `projection ` _(Number[16])_ ProjectionMatrix

Listen to modelView Matrix updates for ALL visible spatial tools. It only works once subscribeToAllMatrices() is called. This listener is synchronized with the video update rate. You can use the callback of this listener to synchronize animations.

```javascript
spatialInterface.addAllObjectMatricesListener(function(allModel,preojection){
 var allModelviewMatrices = allModel; // object of matrices arrays
 var projectionMatrix = preojection;
});
```

#### addDevicePoseMatrixListener(callback)
- `callback ` _(Function)_ This callback is syncronized with video frame rate.
- **Returns** `devicePose ` _(Number[16])_ DevicePoseMatrix 
- **Returns** `projection ` _(Number[16])_ ProjectionMatrix

Listen to device pose Matrix updates from your device. It only works once the related subscription is called. This listener is synchronized with the video update rate.

```javascript
spatialInterface.addDevicePoseMatrixListener(function(devicePose, projection){
  var devicePoseMatrix = devicePose;
  var projectionMatrix = projection;
});
```

#### addScreenPositionListener(callback)
- `callback ` _(Function)_ This callback is synchronized with the video frame rate.
- **Returns** `devicePose ` _(Object)_ Object with x _(Number)_ and y _(Number)_ coordinates

Listen to the 2D screen position for any spatial tool. It only works once the related subscription is called. This listener is synchronized with the video update rate.

```javascript
spatialInterface.addScreenPositionListener(function(position){
    var x = position.x;
    var y = position.y;
});
```

#### cancelScreenPositionListener()
Remove all matrix listeners.

```javascript
spatialInterface.cancelScreenPositionListener();
});
```

#### getPositionX(), getPositionY(), getPositionZ()
- **Returns** _(Number)_ x,y or z

Returns a number for translation distance and position between the spatial Tool and the device.

```javascript
x =  spatialInterface.getPositionX();
y =  spatialInterface.getPositionY();
z =  spatialInterface.getPositionZ();
```

#### getProjectionMatrix()
- **Returns** _(Number[16])_ projection matrix 

Returns the current projection Matrix in the format m[16]

```javascript
projectionMatrix =  spatialInterface.getProjectionMatrix();
```

#### getModelViewMatrix()
- **Returns** _(Number[16])_ model-view matrix

Returns the current modelView Matrix in the format m[16]

```javascript
modelViewMatrix =  spatialInterface.getModelViewMatrix();
```

#### getGroundPlaneMatrix()
- **Returns** _(Number[16])_ ground-plane matrix 

Returns the current ground plane Matrix in the format m[16]

```javascript
groundPlane =  spatialInterface.getGroundPlaneMatrix();
```

#### getDevicePoseMatrix()
- **Returns** _(Number[16])_ device-pose matrix

Returns the current device pose Matrix in the format m[16]

```javascript
devicePose =  spatialInterface.getDevicePoseMatrix();
```

#### getAllObjectMatrices()
- **Returns** _(Object)_ All matrices

Returns all current modelView Matrices in the format `{"objectUuid": number[16], ...}`

```javascript
allMatrices =  spatialInterface.getAllObjectMatrices();
```
<a name="control"></a>
### Control AR Screen Position
Spatial Tools can exist of a simple HTML page or webGL content. Some tools require fullscreen modes or persist in space, and even the related Object is not visible. WebGL content always requires a fullscreen mode since the WebGL context takes over the spatial transformations.

#### setFullScreenOn()
Set your tool to fullscreen mode. Use this non spatial 2D UIs or any WebGL context.

```javascript
spatialInterface.setFullScreenOn();
```
#### setFullScreenOff()
Switches the full screen mode off and attaches the tool back to its spatial position.

```javascript
spatialInterface.setFullScreenOff();
```
#### setStickyFullScreenOn()
Set your tool to permanent fullscreen mode. Use this mode for 2D UI screens, so that the UI does not dissapear when the attached object is out of view. 

```javascript
spatialInterface.setStickyFullScreenOn();
```

#### setStickinessOff()
Remove the stickyness from the Fullscreen mode.

```javascript
spatialInterface.setStickinessOff();
```

#### setExclusiveFullScreenOn()
Set your Tool to permanent fullscreen mode. This mode will remove any other fullscreen mode that is called with the same exclusivity. The callback is called when the fullscreen mode is killed.

```javascript
spatialInterface.setExclusiveFullScreenOn(callback);
```

#### setExclusiveFullScreenOff()
Switches the exclusive full-screen mode off and attaches the Tool back to its spatial position.

```javascript
spatialInterface.setExclusiveFullScreenOff();
```
<a name="media"></a>
### Media Content
These functions help to record videos, images, or use the device build-in keyboard.

#### startVideoRecording()
The Spatial Toolbox will start recording the screen in the background. The final video will automatically be saved once the video is stoped and attached to your spatial Tool.

```javascript
spatialInterface.startVideoRecording();
```

#### stopVideoRecording(callback)
- `callback ` _(Function)_ The callback provides the video URL
- **Returns** url _(String)_ The URL for the video

Stop the video recording and attach it to your Tool.

```javascript
spatialInterface.startVideoRecording(function(url){
    var videoURL = e;
});
```

#### announceVideoPlay()
On certain Mobile Devices, the playback of multiple videos in space can be difficult. Use this function to announce that a single video is played. 

```javascript
spatialInterface.announceVideoPlay();
```

#### subscribeToVideoPauseEvents(callback)
- `callback ` _(Function)_

Use this event to program your video player to pause when other videos are playing. In the best case, try to unload your video and replace it with a screenshot to save resources for other Spatial Tools.

```javascript
spatialInterface.subscribeToVideoPauseEvents(function(){
    // called when pause is requested.
});
```

#### getScreenshotBase64(callback)
- `callback ` _(Function)_
- **Returns** image _(String)_ Image encoded in base64 string. 

Screenshot of the Vuforia Camera. A single frame returned as a base64 string.

```javascript
spatialInterface.getScreenshotBase64(function(image){
    var imageBase64String = image;
});
```

#### openKeyboard()
Open the Keyboard

```javascript
spatialInterface.openKeyboard();
```

#### closeKeyboard()
Close the Keyboard

```javascript
spatialInterface.closeKeyboard();
```

#### onKeyboardClosed(callback)
- `callback ` _(Function)_

Callback if the keyboard is closed

```javascript
spatialInterface.onKeyboardClosed(function(){
    // called on keyboard closed
});
```

#### onKeyUp(callback)
- `callback ` _(Function)_
- **Returns** key _(Object)_ Keybord event

Use this to listen to keyboard entries. The callback returns the entire keyboard event.


```javascript
spatialInterface.onKeyUp(function(key){
    var keyboardEvent = key;
});
```
<a name="behavior"></a>
### Tool Behavior 


#### setVisibilityDistance(distance)
- `distance ` _(Number)_ number in meter to define distance

Define the distance (in meter). Your spatial Tool is visible in the Spatial Toolbox. A user can change this number via the Spatial Toolbox UI.

```javascript
spatialInterface.setVisibilityDistance(2);
```

#### addVisibilityListener(callback)
- `callback ` _(Function)_
- **Returns** key _(Object)_ Keybord event

A callback to read the visibility of a spatial tool. The Interface stays active for 3 seconds after it becomes invisible.

```javascript
spatialInterface.addVisibilityListener(function(visible){
  var isVisible = visible;
});
```

#### getVisibility()
- **Returns** _(Boolean)_

Returns true or false if the spatial Tool is visible.

```javascript
spatialInterface.getVisibility();
```

#### setMoveDelay(delay)
- `delay ` _(Number)_ number in milliseconds

Set how long it takes (in ms) for a user to tap and hold a spatial tool until it becomes moveable. 

```javascript
spatialInterface.setMoveDelay(10);
```

#### addIsMovingListener(callback)
- `callback ` _(Function)_
- **Returns** move _(Boolean)_

Returns true if a tool is moved and false once it is released.

```javascript
spatialInterface.addIsMovingListener(function(move){
  var isMoving = move;
});
```


#### enableCustomInteractionMode()
One way to define when a Tool becomes moveable is the `moveDelay` described above. Another way is to define specific areas that are interactive elements, and as a consequence touching all other areas makes the Tool instantly movable. Call this function to invoke this functionality. Assign all your interactive elements the class `spatial interaction` to make them interactive elements.

```javascript
spatialInterface.enableCustomInteractionMode();
```

#### enableCustomInteractionModeInverted()
This does the invert to the function above. In this case, the elements defined by the next function become the item that allows the spatial moving.

```javascript
spatialInterface.enableCustomInteractionModeInverted()
```

#### setInteractableDivs(divList)
- `divList ` _(String[])_ Array of interactive html elements

Once `enableCustomInteractionMode`or `enableCustomInteractionModeInverted`use this function to define the interactive elements.



```javascript
  spatialInterface.enableCustomInteractionMode();
  spatialInterface.setInteractableDivs([touchInteractiveElement]);
```

#### subscribeToFrameCreatedEvents(callback)
- `callback ` _(Function)_
- **Returns** toolUuid _(String)_ The Uuid of the tool created
- **Returns** type _(String)_ the type of the tool (e.g., graph, slider, etc.)


Triggers a callback anytime another new tool is created while this tool is loaded in the DOM.

```javascript
spatialInterface.subscribeToFrameCreatedEvents(function(toolUuid, type){
var thisToolIsNew = toolUuid;
var itsTypeIs = type;
});
```

#### subscribeToFrameDeletedEvents(callback)
- `callback ` _(Function)_
- **Returns** toolUuid _(String)_ The Uuid of the tool deleted
- **Returns** type _(String)_ the type of the frame (e.g., graph, slider, etc.)

Triggers a callback anytime a tool is deleted while this tool is loaded in the DOM.

```javascript
spatialInterface.subscribeToFrameDeletedEvents(function(toolUuid, type){
var thisToolIsDeleted= toolUuid;
var itsTypeWas = type;
});
```

#### ignoreAllTouches(newValue)
- `newValue` _(Boolean)_

Pass in true (or omit the argument) to make the Tool set the pointer-events to none, so all touches pass through un-altered

```javascript
spatialInterface.ignoreAllTouches(false);
```

#### registerTouchDecider(callback)
- `callback ` _(Function)_ A callback function that decides if our Tool is touched or not. 
- **Returns** eventData _(Object)_ touch event includes x and y of screen touch.
- **Returns** _(Boolean)_ Your callback should return a boolean to decide if a touch is inside an element or outside.

Use this function in a fullscreen tool for when you use, for example, uses threejs or similar. You need to pass a function that decides if a touch event is consumed with your experience.

```javascript
spatialInterface.registerTouchDecider(function(eventData){
  return doSomethingThatDecides(eventData.x, eventData.y);
});
```

#### unregisterTouchDecider()
 Cancel the touch decider callback.
 
```javascript
spatialInterface.unregisterTouchDecider();
```

#### changeFrameSize(newWidth, newHeight)
- `newWidth` _(Number)_ A new touch overlay width
- `newHeight ` _(Number)_ A new touch overlay height

Adjust the size of the Tool's touch overlay element to match the current size of this tool.

```javascript
spatialInterface.changeFrameSize(640, 480);
```

#### getScreenDimensions(callback)
- `callback ` _(Function)_
- **Returns** _(Number)_ screen dimension width
- **Returns** _(Number)_ screen dimension height

Asynchronously query the screen width and height from the parent application, as the iframe itself can't access that.

```javascript
spatialInterface.registerTouchDecider(function(width, height){
    screenDimensionWidth = width;
    screenDimensionHeight = height;
});
```

<!---
#### addInterfaceListener
What menu button do I push right now. 
callback (menu button string)

#### getInterface         
returns what interface is activated
--->
      

<a name="edgeComm"></a>
### Spatial Edge Server Communication 
The previous function handles the communication and behavior within the Spatial Toolbox. The following functions handle the communication with the Spatial Edge Server that owns the Object in which the Spatial Tool is registered.

#### write(node, value, mode, unit, unitMin, unitMax, forceWrite)
- `node ` _(String)_ Name of Node
- `value ` _(Number)_ The new value for the Node. The range <u>must</u> be between 0.0 and 1.0.
- `mode ` _(String)_ [optional] Data mode. Currently, `f` for float is the only supported choice.
- `unit ` _(String)_ [optional] The unit of your data such as 'kg', 'minutes', 'liter', ... 
- `unitMin ` _(String)_ [optional] value range minimum
- `unitMax ` _(String)_ [optional] data range maximum
- `forceWrite ` _(Boolean)_ [optional] Sends the Value even if it hasn't changed since the last write.


Write flow data to a node owned by the Spatial Tool. This flow data is of a simple floating data type with values between 0.0 and 1.0.
The example below, writes 0.5 to the `node` = "nodeName", `unit` = kg, `min` = 0, `max` = 10 and the last entry tells that the Value is only updated on change. The resulting value will be 5 kg. 

```javascript
spatialInterface.write("nodeName", 0.5, "f", "kg", 0, 10, false);
```

#### getUnitValue(flowDataObject)
- `flowDataObject ` _(String)_ flow data object
- **Returns** _(Object)_ real value (mapped 0 - 1 to min - max) and unit

Returns the real Value and Unit of a node flow data object. The Value is mapped to the minimum and maximum. For example a data package with a value of 0.5, a min = 0, max = 10 and unit = "kg" will return a value of 5 and the unit "kg".

```javascript
valueAndUnitObject = spatialInterface.getUnitValue(flowDataObject)
```

#### spatial.addReadListener(node, callback)
- `node ` _(String)_ name of Node
- - `callback ` _(Function)_ is called anytime the server provides a new data object for the Node.
- **Returns** flowDataObject _(Object)_ flow data object for the Node

Add a read listener to read value changes from a node. These changes are synchronized with the Edge Server for as long as the Spatial Tool active. The Tool becomes inactive 3 seconds after it's not visible anymore. 

```javascript
spatialInterface.addReadListener("nodeName", function(flowDataObject){
    var realValues = spatialInterface.getUnitValue(flowDataObject);
    value = realValues.value;
    unit = realValues.unit;
});
```

#### readRequest(node)
- `node ` _(String)_ name of Node

This forces the server to send the latest Value so that the readListener can pick it up. You can use this in combination with addReadListener to initialize your spatial Tool if needed.

```javascript
spatialInterface.readRequest("nodeName");

spatialInterface.addReadListener("nodeName", function(e){
    var package = spatialInterface.getUnitValue(e);
    value = package.value;
    unit = package.unit;
});
```


#### writePublicData(node, valueName, value, realtimeOnly) 
- `node ` _(String)_ name of Node
- `valueName ` _(String)_ public data is defined by the Node itself. This is a key to select an object within public Data.
- `value ` _(Object/String/Number)_ Data you want to store with a key.
- `realtimeOnly ` _(Boolean)_ doesn't send a post message to reload Object, allows rapid stream.


PublicData is a JSON object that contains data objects specified by the server-side node definition. You need to finetune your Tool via specific node types. The publicData allows you to handle complex data among the server-side node program, and the Spatial Tool and the Value can contain any JSON encoded data. The last parameter is optional and if set to true, forces the server not to store the data but handle it as realtime flow.

```javascript
spatialInterface.writePublicData("nodeName", "key", anyValue, false);
```

#### writePrivateData(node, valueName, value)
- `node ` _(String)_ name of Node
- `valueName ` _(String)_ writePrivateData is defined by the Node itself. This is a key to select an object within private Data.
- `value ` _(Object/String/Number)_ Data you want to store with the key.

Unlike publicData, privateData is write-only, and its content is only read access to the edge server. Spatial Tools and the Spatial Toolbox will never have read access.

```javascript
spatialInterface.writePublicData("nodeName", "key", anyValue);
```

#### readPublicData(node, valueName, value)
- `node ` _(String)_ name of Node
- `valueName ` _(String)_ This is a key to select an object within publicData.
- `value ` _(Number)_ [Optional] If Value is undefined, this default value is used.
- **Returns** _(Object/String/Number)_ returns the stored data.

Read data from publicData once. Use this function if you need to fill your spatial Tool with all current publicData values. The last parameter is optional and defines a default value in case the key does not yet exist in the publicData.

```javascript
value = spatialInterface.readPublicData("nodeName", "key", value);
```

#### addReadPublicDataListener(node, valueName, callback)
- `node ` _(String)_ name of Node
- `valueName ` _(String)_ This is a key to select an object within publicData.
- `callback ` _(Function)_
- **Returns** value _(Object/String/Number)_ returns the stored data.


Add a read listener for publicData to read value changes for a defined Key. These changes are synchronized with the Edge Server for as long as the Spatial Tool active. The Tool becomes inactive 3 seconds after it's not visible anymore. 

```javascript
spatialInterface.addReadPublicDataListener("nodeName", "key", function(value){
var publicDataValue = value;
});
```

#### reloadPublicData()

Sends a message to the edge server to send the most recent state of publicData. You can use this to initialize your spatial Tool, since the publicDataListener only reacts to updates. 

```javascript
spatialInterface.reloadPublicData();
spatialInterface.addReadPublicDataListener("nodeName", "key", function(value){
    var publicDataValue = value;
});
```
