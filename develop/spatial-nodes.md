---
layout: doc
title: Create New Nodes
permalink: /docs/develop/spatial-nodes
---

<a name="newNode"></a>
## Create new Nodes
The Node is the central data storing and data processing element within the Spatial Toolbox. You can define new nodes that your hardware Interface and your spatial Tool can use. Each Node needs to have a specific structure to function.

#### Folder Structure
Your node-addon should be stored in the addons/nodes/[yourNodeAddon]/ folder, and it requires two files as a default:

- `index.js` stores all the logic for your Node.
- `gui/index.html` This stores the visible appearance for the Node

<a name="nodeIndex"></a>
###index.js

#### generalProperties
- `generalProperties` _(Object)_ defines the data parameters for the new Node.
- `name` _(String)_ The name for the Node
- `privateData` _(Object)_ Use this Object to define private data. The Spatial Toolbox can only write content to this Object but it can not read content.
- `publicData` _(Object)_ Use this Object to define additional data types that your Node requires. Keys need to be defined in this Object so that the Spatial Toolbox can access them.
- `type` _(String)_ Type should match the folder name.


#### setup(object, tool, node, thisNode)
- `object` _(String)_
- `tool` _(String)_
- `node` _(String)_
- `thisNode` _(Object)_




#### render(object, tool, node, thisNode, callback)
- `object` _(String)_ Currently processed Object UUID
- `tool` _(String)_ Currently processed Tool UUID
- `node` _(String)_ Currently processed Node UUID
- `thisNode` _(Object)_ Reference to the node Object
- `callback ` _(Object)_ callback for when the data flow processing engine should continue.
- **Returns** `object` _(String)_ Currently processed Object UUID
- **Returns** `tool` _(String)_ Currently processed Tool UUID
- **Returns** `node` _(String)_ Currently processed Node UUID
- **Returns** `thisNode` _(Object)_ Reference to the node Object

The Edge Server will call this function on any tick. It will provide the Object, Tool, node UUIDs as well the entire node Object as ReferenceReference. In case you want your Node to act as an input and output, you must call the callback function so that an output event is triggered. You can call this callback asynchronous at any time.

All in all, the node logic has four parts:

1. generalProperties act as a data storage
2. setup executes a single time
3. render is called by the edge server whenever a new tick triggers trough the logic.
2. spatial data that is saved by the Edge Server and edited via the Spatial Toolbox. This spatial data is accessible via `thisNode` reference.


Example of a delay node:

```javascript
var generalProperties = {
    name: 'delay',
    privateData: {},
    publicData: {delayTime: 1000},
    type: 'delay'
};

exports.properties = generalProperties;

exports.setup = function (object, tool, node, nodeProperties) {
// add code here that should be executed once.
};

exports.render = function (object, tool, node, thisNode, callback) {
    var delayedValue = thisNode.data.value;

    setTimeout(function() {
        thisNode.processedData.value = delayedValue;
        callback(object, tool, node, thisNode);
    }, thisNode.publicData.delayTime);

};
```
<a name="nodeGuiIndex"></a>
###gui/index.html
