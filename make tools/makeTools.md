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
##### sendEnvelopeMessage
##### sendCreateNode
##### sendMoveNode
##### sendResetNodes

##### addGlobalMessageListener
Allows you to listen to messages send to all other spatial tools currently visible in the Spatial Toolbox.

```javascript
spatialTool.addGlobalMessageListener(function(e){
  console.log(e);
});
```
##### addFrameMessageListener



### Subscribe to Matrices


##### subscribeToMatrix
##### subscribeToScreenPosition
##### subscribeToDevicePoseMatrix
##### subscribeToAllMatrices
##### subscribeToGroundPlaneMatrix
##### subscribeToAcceleration

##### addMatrixListener
Listen to 3D-transformations updates. It only works once subscribeToMatrix() has been called. This listener is synchronized with the video update rate.

```javascript
spatialTool.addMatrixListener(function(e,f){
  modelview = e;
  projection = f;
});
```

##### addAllObjectMatricesListener
##### addDevicePoseMatrixListener
##### addScreenPositionListener
##### cancelScreenPositionListener
// deprecated or unimplemented methods
##### addAccelerationListener

##### getPosition

Returns a number for translation distance and position between the iOS device and the marker.
```javascript
x =  spatialTool.getPositionX();
y =  spatialTool.getPositionY();
z =  spatialTool.getPositionZ();
```

##### getProjectionMatrix
##### getModelViewMatrix
##### getGroundPlaneMatrix
##### getDevicePoseMatrix
##### getAllObjectMatrices

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
##### setStickinessOff
##### setExclusiveFullScreenOn
##### setExclusiveFullScreenOff

### Media Content

##### startVideoRecording
##### stopVideoRecording
##### getScreenshotBase64
##### openKeyboard
##### closeKeyboard
##### onKeyboardClosed
##### onKeyUp

### Tool Behavour 

##### setMoveDelay
##### setVisibilityDistance
##### activateScreenObject
##### enableCustomInteractionMode
##### enableCustomInteractionModeInverted
##### setInteractableDivs
##### subscribeToFrameCreatedEvents
##### subscribeToFrameDeletedEvents
##### announceVideoPlay
##### subscribeToVideoPauseEvents
##### ignoreAllTouches
##### changeFrameSize
// deprecated methods
##### sendToBackground

##### addVisibilityListener
Allows you read if the interface is visible or not. The interface stays active for 3 seconds after it becomes invisible.
```javascript
spatialTool.addVisibilityListener(function(e){
  visible = e;
});
```

##### addInterfaceListener
##### addIsMovingListener

##### getVisibility


##### getInterface         
      
##### getUnitValue
##### getScreenDimensions
##### getMoveDelay
// deprecated getters
##### search

##### registerTouchDecider
##### unregisterTouchDecider
          

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
##### addReadPublicDataListener 
##### writePublicData 
##### reloadPublicData 
##### addScreenObjectListener
##### addScreenObjectReadListener
// deprecated or unimplemented methods
##### read
##### readRequest
##### writePrivateData
##### initNode

    



## Create new Interfaces

### API Reference


##### write
##### writePublicData
##### clearObject
##### removeAllNodes
##### reloadNodeUI
##### getAllObjects
##### getKnownObjects
##### getAllFrames
##### getAllNodes
##### getAllLinksToNodes
##### subscribeToNewFramesAdded
##### runFrameAddedCallbacks
##### subscribeToReset
##### runResetCallbacks
##### subscribeToMatrixStream
##### triggerMatrixCallbacks
##### subscribeToUDPMessages
##### triggerUDPCallbacks
##### addNode
##### renameNode
##### moveNode
##### removeNode
##### attachNodeToGroundPlane
##### pushUpdatesToDevices
##### activate
##### deactivate
##### getObjectIdFromObjectName
##### getMarkerSize
##### enableDeveloperUI
##### getDebug
##### setup
##### reset
##### readPublicDataCall
##### screenObjectCall
##### screenObjectServerCallBack
##### addScreenObjectListener
##### writeScreenObjects
##### activateScreen
##### getScreenPort
##### addReadListener
##### addPublicDataListener
##### connectCall
##### addConnectionListener
##### removeReadListeners
##### map
##### addEventListener
##### advertiseConnection
##### shutdown
##### loadHardwareInterface


## Create new Nodes

## Create new Blocks

