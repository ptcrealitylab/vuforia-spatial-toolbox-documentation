<a name="newBlock"></a>
## Create new Blocks
The Block is similar to a node. However, there are significant differences:

1. The Block is an element within a Logic Crafting Board. The Logic Crafting Board is a very simple data flow programming system that allows for spatial programming.
2. As such, blocks have the spatial information of their parent logic Node. However, they do not own a spatial component themselves. 
1. The Block expands the simple data flow from the Node to four possible in and outputs.
2. The Block requires some additional files within the `gui` folder for it to work within the logic crafting board. 

#### Folder Structure
Your block-addon should be stored in the addons/nodes/[yourBlockAddon]/ folder, and it requires the following files:

- `index.js` stores all the logic for your Block.
- `gui/index.html` This stores a setup screen that you can use for adjusting the properties for your Block stored in `publicData`. 
- `gui/icon.svg` This SVG image will appear as an icon in the Spatial Toolbox block selection UI. 
- `gui/label.svg` The Label SVG Image will visualize the Block within the Logic Crafting Board.

<a name="blockIndex"></a>
###index.js

#### generalProperties
- `generalProperties` _(Object)_ defines the data parameters for the new Node.
- `name` _(String)_ name is displayed underneath icon in block menu
- `blockSize` _(Number)_ Defines the width for your Block. It also defines the number of in and outputs.
- `privateData` _(Object)_ Use this Object to define private data. The Spatial Toolbox can only write content to this Object but it can not read content.
- `publicData` _(Object)_ Use this Object to define additional data types that your Node requires. Keys need to be defined in this Object so that the Spatial Toolbox can access them.
- `activeInputs` _(Boolean[4])_ Sets which input indices of the block can have links drawn to them
- `activeOutputs`_(Boolean[4])_ Sets which output indices of the Block can have links drawn from them
- `nameInput`_(String[4])_ Sets a name for every input
- `nameOutput`_(String[4])_ Sets a name for every output
- `type` _(String)_ Type should match the folder name.


#### setup(object, tool, node, thisNode)
- `object` _(String)_
- `tool` _(String)_
- `node` _(String)_
- `block` _(String)_
- `thisNode` _(Object)_




#### render(object, tool, node, thisNode, callback)
- `object` _(String)_ Currently processed Object UUID
- `tool` _(String)_ Currently processed Tool UUID
- `node` _(String)_ Currently processed Node UUID
- `block` _(String)_ Currently processed Block UUID
- `thisNode` _(Object)_ Reference to the node Object
- `callback ` _(Object)_ callback for when the data flow processing engine should continue.
- **Returns** `object` _(String)_ Currently processed Object UUID
- **Returns** `tool` _(String)_ Currently processed Tool UUID
- **Returns** `node` _(String)_ Currently processed Node UUID
- **Returns** `block` _(String)_ Currently processed Block UUID
- **Returns** `thisNode` _(Object)_ ReferenceReference to the node Object

The Edge Server will call this function similar to the Node on any tick. It will provide the Object, Tool, Node, and Block UUIDs as well the entire block Object as Reference. In case you want your Block to act as an input and output, you must call the callback function so that an output event is triggered. You can call this callback asynchronous at any time.

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

exports.setup = function (object, tool, node, block, thisBlock, callback) {
    // add code here that should be executed once.
};

exports.render = function (object, tool, node, block, index, thisBlock, callback) {
    var delayedValue = thisBlock.data[index].value;

    setTimeout(function() {
        thisBlock.processedData[index].value = delayedValue;
        callback(object, tool, node, block, index, thisBlock);
    }, thisBlock.publicData.delayTime);

};
```
<a name="blockGuiIndex"></a>
###gui/index.html

<a name="blockGuiIcon"></a>
###gui/icon.svg

<a name="blockGuiLabel"></a>
###gui/label.svg
