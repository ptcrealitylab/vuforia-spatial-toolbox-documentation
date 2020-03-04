# Folder Structure

<img align="right" width="150" height="400" src="folder.svg">
sdasdsajlkdhaslkdjhdsflkajsdfhlekjfh
<br clear="right"/>

# Create new Tools

## API Reference


#### Initialize the Javacript Library within your Spatial Tool
Include the Spatial Toolbox functionality by instancing as following in your Java Script code:

```javascript
var spatialTool = new SpatialTool();
```

### Communication with other Spatial Tools


##### sendGlobalMessage
Send broadcast messages to all other spatial tools currently visible in the Spatial Toolbox.

```javascript
spatialTool.sendGlobalMessage("Hello World");
```
##### sendMessageToFrame
Message to a specific frame via frameKey
```javascript
spatialTool.sendGlobalMessage(frameKey, "Hello World");
```

##### sendCreateNode
Create a node on that frame
name, x, y, attachTogroundplane (boolean), type. noDoublicate (boolean)

##### initNode
Create a node on that frame

name , type , x, y, scale, default

##### sendMoveNode
Move a node around
name, x,y

##### sendResetNodes
Removes all nodes from frame
()

##### addGlobalMessageListener
Allows you to listen to messages send to all other spatial tools currently visible in the Spatial Toolbox.

```javascript
spatialTool.addGlobalMessageListener(function(e){
  console.log(e);
});
```
##### addFrameMessageListener
Only listens to send message to frame API

```javascript
spatialTool.addFrameMessageListener(function(e){
  console.log(e);
});
```

### Subscribe to Matrices


##### subscribeToMatrix
Tell the toolbox to send matrices
()

##### subscribeToScreenPosition
Tell the 2D screen position of a 3D element 
()

##### subscribeToDevicePoseMatrix
Subscribe the matrix where the phone is
()

##### subscribeToAllMatrices
Give you all visible objects matrices of all visible frames in the scene
() 

##### subscribeToGroundPlaneMatrix
Easy Ground plane 

##### addMatrixListener
Listen to 3D-transformations updates. It only works once subscribeToMatrix() has been called. This listener is synchronized with the video update rate.

```javascript
spatialTool.addMatrixListener(function(e,f){
  modelview = e;
  projection = f;
});
```

##### addAllObjectMatricesListener
```javascript
spatialTool.addMatrixListener(function(e,f){
  modelview = e; // object of matrices arrays
  projection = f;
});
```

##### addDevicePoseMatrixListener
```javascript
spatialTool.addMatrixListener(function(e,f){
  modelview = e;
  projection = f;
});
```

##### addScreenPositionListener
```javascript
spatialTool.addMatrixListener(function(e){
  modelview = e;
object with x and y property
});
```

##### cancelScreenPositionListener
Kill screen position listener

##### getPosition

Returns a number for translation distance and position between the iOS device and the marker.
```javascript
x =  spatialTool.getPositionX();
y =  spatialTool.getPositionY();
z =  spatialTool.getPositionZ();
```

##### getProjectionMatrix
Require subscription 

return a matrix array 

##### getModelViewMatrix
Require subscription 

return a matrix array 

##### getGroundPlaneMatrix
Require subscription 

return a matrix array 

##### getDevicePoseMatrix
Require subscription 

return a matrix array 

##### getAllObjectMatrices
Require subscription 

return an object of matrix array 


### Control AR Screen Position

##### setFullScreenOn
Forces the Reality Editor to show your content full screen without 3d transformation. This comes in handy for when you want to use the transformation matrices directly.
```javascript
spatialTool.setFullScreenOn();
```
##### setFullScreenOff
Does the opposite from setFullScreenOn.
```javascript
spatialTool.setFullScreenOff();
```
##### setStickyFullScreenOn
Thats clear like fullscreen but sticky

##### setStickinessOff
just remove stickiness

##### setExclusiveFullScreenOn
Only have this one on and only kills others with the same call

callback, triggered if killed.

##### setExclusiveFullScreenOff
killing the fullscreen

### Media Content

##### startVideoRecording
App will start recording background. It will automatically be saved on the server for you.
It will be saved if you call stop video recording

##### stopVideoRecording
Stop video recording will save automatically to the server

callback gives you the URL for where the video is saved. 


##### getScreenshotBase64
Save image of the screen

callback with a Base64 String

##### openKeyboard
open keyboard to write stuff 
listen to key pressed
()

##### closeKeyboard
closes the keyboard again 
()
##### onKeyboardClosed
callback if keyboard is closed

##### onKeyUp
Gives you what key was pressed

callback with full keyboard event.

### Tool Behavour 

##### setMoveDelay
Set how long it takes for a person to tap until a tool becomes moveable.
(delay in ms)


##### setVisibilityDistance
(meters of visiblity)
Set how far away something is visible

##### enableCustomInteractionMode
YOu call this function to specify what becomes interactable and what moves the tool
Touches emediatly move frame around
unless you touch on a div that has a specific class called "spatial interaction"

##### enableCustomInteractionModeInverted
If you want nothing but one thing to move the frame around

##### setInteractableDivs
once the one before is set, you can give this the IDs of the divs that are interactable.

(divList [array of actual div])

##### subscribeToFrameCreatedEvents

callback anytime the user creates a new frame while this frame is visible
frameID, frame type inside object

##### subscribeToFrameDeletedEvents
same callback but if deleted

##### announceVideoPlay
I play you stop 
()

##### subscribeToVideoPauseEvents
subcribe to the anounceVideoPlay event. You have to implement your player to follow the call.

##### ignoreAllTouches

Tells Tools to do pointer events none you can not do anything with it.
(true/false)


##### changeFrameSize

Updating the tools knowlage of the size of an object
(with, height)


##### addVisibilityListener
Allows you read if the interface is visible or not. The interface stays active for 3 seconds after it becomes invisible.
```javascript
spatialTool.addVisibilityListener(function(e){
  visible = e;
});
```

##### addInterfaceListener
What menu button do I push right now. 
callback (menu button string)


##### addIsMovingListener

callback (currently tragging frame around) boolean 

##### getVisibility
tell if visible boolean 

##### getInterface         
returns what interface is activated
      
##### getUnitValue
helper function for read package from server to get unit of value regarding the unit 

returns value and unit in object

##### getScreenDimensions
get screen dimensions of the actual device. threejs needs it to set aspect

callback (with, heigh)

##### registerTouchDecider

For threejs to know if you tap something that should count as touched. 

You hand over a function whos job is to return true or falls in case something is hit or not.

(hand over function) In fullscreen frames

##### unregisterTouchDecider
 cancel the touch decider. switch between fullscreen and not full screen
 ()

### Spatial Edge Server Communication 


##### write

You can write to the Hybrid Object with write(). The scale of your values should be between 0.0 and 1.0.

```javascript
spatialTool.write("led", output);
```
##### spatial.addReadListener

Every communication with the object is happening passively. This means that the object is not pushing data. If a user interface wants to read data from a Hybrid Object it first needs to send a read request.

```javascript
spatialTool.addReadListener("led", function(e){
  input = e*255;
});
```

##### readPublicData 
Read the public data object 
(node, valueName, callback)
content of that specific item within the data

##### addReadPublicDataListener 
when ever public data changes 
(node, valueName, callback)
content of that specific item within the data

##### writePublicData 
(node, valueName, value) (realTimeonly (optional)) but no write to server just in time realtime
content of that specific item within the data

##### reloadPublicData 
emits a message to the server to request to send most recent
()

##### readRequest
force to send data
()

##### writePrivateData
write private Data 

node, valueName, Value

    



## Create new Interfaces

### API Reference


##### write
write data to a specific object frame node

objectName, frameName, nodeName, value, mode, unit, min, max

##### writePublicData
objectName, frameName, nodeName, dataObject (value name), data

##### clearObject
Older APIS remove IO points which are no longer needed. 
Deletes all nodes 

ObjectID, Frame ID

##### removeAllNodes
Remove all nodes via Name and frame name 

##### reloadNodeUI
reload this object within all open editors... important to refresh the tool in the toolbox
(objectName)


##### getAllObjects
returns all objects object

##### getKnownObjects
get all known object among the entire network
object of objects IDs 

{uuid {version, protocol, ip}}

##### getAllFrames
(objectName)
return all frames that this object has 

##### getAllNodes
{objectName, frameName)

just the nodes

##### getAllLinksToNodes
objectname, framename
returns all links known to frame

##### subscribeToNewFramesAdded
object name, callback 
it will triger tallback anytime frame gets added to object. 
callback(frame)

##### subscribeToReset
Screen hardware interface 
if git reset commit is pushed this can help to reinit

callback();


##### subscribeToUDPMessages

you can listen to action events trough this

msgContent 

action messages for example


##### addNode
adds a node

objectNmae, frameName, NodeName, type, possition (){x,y}


##### renameNode
objectNmae, frameName, old nodeName, newNodeName

##### moveNode
objectNmae, frameName, NodeName, x,y,scale, matrix, loyalty (optional)

##### removeNode
objectNmae, frameName, NodeName,

##### attachNodeToGroundPlane
objectNmae, frameName, NodeName, boolean

##### pushUpdatesToDevices
objectName forces the toolboxes to reload

##### activate
activates an object
()
##### deactivate
deactivates an object
()
##### getObjectIdFromObjectName
Get UUID of Object via Name

Name return UUID
##### getMarkerSize
objectName return getMarkerSize


##### addReadListener
objectNmae, frameName, NodeName, callback (dataObject)

##### addPublicDataListener
objectNmae, frameName, NodeName, dataObject(valueListen for), callback (publicData)
what ever public data is


##### addConnectionListener
objectNmae, frameName, NodeName, callback (whole link structure constructor)
 
 subscribe if link is created


##### removeReadListeners
objectNmae, frameName, NodeName,
() remove a read listner 

##### map
value, in min, in max, out min, out max

##### addEventListener
reset and shutdown

(option string (callback))

Closing serial ports and stuff to 

##### advertiseConnection
(describe this later on.) Cool functionality but maybe overwelming for now


##### loadHardwareInterface
(_dirName)
HardwareInterfaceName
settings.json file and return json content 

## Create new Nodes

## Create new Blocks

