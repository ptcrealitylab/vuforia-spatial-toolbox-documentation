---
layout: doc
title: Add-ons
permalink: /docs/develop/add-ons
---

## Add-ons

As a developer, your main option for contributing to and extending the Vuforia Spatial Toolbox with
new functionality is using the add-on system. Add-ons let you build your choice of five different
types of capabilities. Anyone who installs your add-on on their edge server will gain the
those capabilities.

The five types of content that an add-on can contain are:

1. *Spatial Tools* – for new AR content
2. *Hardware Interfaces* – to connect to new external systems
3. *Spatial Nodes* – for new ways of processing data in space
4. *Spatial Blocks* – for new logical operators in the programming system
5. *Content Scripts* - an advanced feature for new scripts to be injected into your app

Add-ons get stored in the `addons` directory of the `vuforia-spatial-edge-server`. Each directory
 within `addons` will be loaded as a separate add-on.

By default, each server should have the `vuforia-spatial-core-addon` installed, which provides
all of the default tools, interfaces, nodes, and blocks necessary for the basic functionality of
the system. If you want to build your own add-on, look at the core-addon for examples.

Each add-on needs a specific structure for the content of each type to be loaded. An add-on with
 every type of content (1–5, above) should have these folders at the top level:

```
tools/
interfaces/
nodes/
blocks/
content_scripts/
```

An add-on doesn't need to have all types of content. For example, you can create an add-on just
to add new tools to the system, without adding interfaces, nodes, blocks, or scripts. In that
case, the top level of your add-on would only have a `tools` directory.

The rest of the Develop section will walk you through the concepts and APIs for creating each of
these kinds of content in an add-on.

We suggest following these in order, but if you're only interested in developing a specific type
of add-on, you can jump to the corresponding section, where we have tutorials and API
 documentation for each type of content:

1. [Spatial Tools](./spatial-tools)
2. [Hardware Interfaces](./hardware-interfaces)
3. [Spatial Nodes](./spatial-nodes)
4. [Spatial Blocks](./spatial-blocks)

There aren't any tutorials on creating content_scripts yet – if you're curious about them, please
reach out on the [forum](https://forum.spatialtoolbox.vuforia.com).
