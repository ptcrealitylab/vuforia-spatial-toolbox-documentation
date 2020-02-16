# Folder Structure

<img align="right" width="150" height="400" src="folder.svg">
sdasdsajlkdhaslkdjhdsflkajsdfhlekjfh
<br clear="right"/>

# Create new Tools

## API Reference


#### Initialize the Javacript Library
Include the open hybrid functionality by instancing as following in your Java Script code:

```javascript
var toolbox = new Toolbox();
```

#### .addReadListener([IO Point], callback)
Every communication with the object is happening passively. This means that the object is not pushing data. If a user interface wants to read data from a Hybrid Object it first needs to send a read request.

Example:
```javascript
toolbox.addReadListener("led", function(e){
  input = e*255;
});
```


#### .write([IO Point], [Value 0 ~ 1])
You can write to the Hybrid Object with write(). The scale of your values should be between 0.0 and 1.0.

Example:
```javascript
toolbox.write("led", output);
```

#### .sendGlobalMessage([message])
Send broadcast messages to all other objects currently visible in the Reality Editor.

Example:
```javascript
toolbox.sendGlobalMessage("Hello World");
```

#### .addGlobalMessageListener(callback[message])
Allows you to listen to messages send to all other objects currently visible in the Reality Editor.

Example:
```javascript
toolbox.addGlobalMessageListener(function(e){
  console.log(e);
});
```

#### .addMatrixListener(callback[modelViewMatrix][porjectionMatrix])
Allows you to listen to updates 3D-transformations. It only works once subscribeToMatrix() has been called. This listener is synchronized with the video update rate.

Example:
```javascript
toolbox.addMatrixListener(function(e,f){
  modelview = e;
  projection = f;
});
```

#### .setFullScreenOn()
Forces the Reality Editor to show your content full screen without 3d transformation. This comes in handy for when you want to use the transformation matrices directly.

Example:
```javascript
toolbox.setFullScreenOn();
```

#### .setFullScreenOff()
Does the oposit from setFullScreenOn. 🙂

Example:
```javascript
toolbox.setFullScreenOff();
```

#### .addVisibilityListener(callback[e])
Allows you read if the interface is visible or not. The interface stays active for 3 seconds after it becomes invisible.

Example:
```javascript
toolbox.addVisibilityListener(function(e){
  visible = e;
});
```

#### .getPositionX(), .getPositionY(), .getPositionZ()
Returns a number for translation distance and position between the iOS device and the marker.

Example:
```javascript
x =  toolbox.getPositionX();
y =  toolbox.getPositionY();
z =  toolbox.getPositionZ();
```

## Create new Interfaces

### API Reference


## Create new Nodes

## Create new Blocks

