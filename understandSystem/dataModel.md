## Object Data Model

You should learn several key vocabulary terms, which correspond to classes or other data structures that compose the system's data model.

1. Object
2. Frame (Tool)
3. Node
4. Link
5. Data
6. Logic Node
7. Block
8. Block Link

### Object

An Object (sometimes referred to as a Reality Object) corresponds to exactly one AR target. When the target is recognized by Vuforia, all data associated with the Object is downloaded from the edge server hosting that Object, and rendered in the app.

Objects are identified by a human readable *name*, as well as a unique *objectId* (`{object name}+{random 12 character string}`). Objects are stored in the user's local `~/Documents/realityobjects` directory, which is loaded when an edge server is started on the machine. The full properties of an Object are stored in `~Documents/realityobjects/{object name}/.identity/object.json`.

There is also a certain type of Object called a **World Object** which doesn't require a target and instead remains visible as you move around a space.

Objects are stored in the userinterface in the `objects` global variable. Objects are stored on edge servers in the `objects` variable in the server.js file.

- Each object can have 0, 1, or many frames...

### Frame (Tool)

The data model for a single piece of AR content, which can be attached to an object. A frame has an iframe element that is rendered while its object's Vuforia target is visible. A frame has 3D position data that can be modified by the client and a src to load into the iframe. iFrame content can interact with AR capabilities using a JavaScript API. A frame can be *local* (permanently designed for, and tied to, its object) or *global* (able to be moved from one object to another).

Like objects, frames also have a *name* and a unique *frameId*. Each local frame has a directory located at `realityobjects/{object name}/{frame name}`, which contains an index.html that is loaded into the frame. Each global frame is stored by name in the object.json file, and its name is resolved into an index.html src by the edge server it belongs to.

The set of available global frames is loaded from the `tools` directory of all edge server addons, such as the [core-addon/tools](https://github.com/ptcrealitylab/vuforia-spatial-core-addon/tree/master/tools).

Frames are stored in the `frames` property of an object.

- Each frame can have 0, 1, or many nodes...
- Each frame can have 0, 1, or many links...

### Node

A node represents a specific input and/or output of a frame, which can be logically connected to nodes of other frames by *linking* them together.

A node stores a data value and has a visual AR representation that will be rendered relative to its frame's position when a user looks at the frame in programming mode. When nodes are linked together, and a new data value arrives at a node, the data value will propagate to any nodes linked downstream from it.

Data can be written to or read from a node, either from a frame's iframe (using the JavaScript API), or from connected hardware (using an edge server's *hardware interface*).

Nodes are stored in the `nodes` property of a frame.

### Link

Links connect a start node to an end node. When data is written to a node, all nodes that it is linked to will receive that data as well. Links are directional. Writing data to an end node will not propagate backwards to the start node. The direction of the link is visualized in the userinterface by the direction of the flowing dots.

Links are stored in the `links` property of a frame.

### Data

A data packet that is sent between nodes that are linked together. It contains a `value` which by default should be formatted as a floating point number within the range of 0 to 1, inclusive. 0 means off, while 1 means on. A data packet can optionally set its `unitMin` and `unitMax` if it is using a different range than [0, 1], so that the value can be normalized and will still be compatible with other nodes. A data packet can also optionally set a `unit` string to tag the data with a unit (such as meters or kg), which certain frames can visualize or convert between.

Exactly one data packet is stored in the `data` property of a node. This is the data that was most recently sent to this node.

### Logic Node

A logic node is a special type of node that can contain a customized program that affects the data flowing through it. The program is assembled out of *blocks* and *block links*. Tapping on a logic node opens up a grid-based visual programming interface where the user can construct such a program. Logic nodes can be linked with regular nodes, with the small difference that logic nodes have four color-coded inputs and four color-coded outputs to choose from. Data enters the logic node through one of its inputs and can pass through multiple logic blocks before exiting one of its outputs. Any of the blocks it passes through will affect the data value that gets outputted.

Logic Nodes are stored in the `nodes` property of a frame, along with the regular nodes. They just have a `{type: "logic"}` property rather than `{type: "node"}`.

- Each logic node can have 0, 1, or many blocks...
- Each logic node can have 0, 1, or many block links...

### Block

A Block (also known as a Logic Block), is a single logical operator that affects data that passing through it, and can be linked together to form more complex programs. Some examples include inverting a value, adding two values together, or waiting a time delay before outputting the value. Blocks must be placed inside a logic node. Blocks can have configurable settings, and can provide UIs to change these, such as the time to delay or a scale factor to multiply by.

The set of available blocks is loaded from the `blocks` directory of all edge server addons, such as the [core-addon/blocks](https://github.com/ptcrealitylab/vuforia-spatial-core-addon/tree/master/blocks).

Blocks are stored in the `blocks` property of a logic node.

### Block Link

A block link is analagous to a link between nodes, except it connects a start block to an end block instead of a start node to an end node.

Block links are stored in the `links` property of a logic node.

