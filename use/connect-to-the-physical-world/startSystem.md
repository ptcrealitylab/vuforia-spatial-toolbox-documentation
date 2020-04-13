---
layout: doc
title: Starting the Vuforia Spatial Edge Server
permalink: /docs/use/connect-to-the-physical-world/startSystem
---

## Connect to the Physical World

If you're just testing the Spatial Toolbox app on your device, you don't need to run any
additional servers or software. If, however, you want AR content to "stick" to images
or objects in the physical world, or if you want to program physical machines with these AR tools,
you need to run a Vuforia Spatial Edge Server from a computer in your network.

## Install Edge Server

If you are an experienced Node.js developer and want to get started
immediately, follow the instructions under "Quick Start for Developers."
Otherwise, scroll onwards to find more in-depth documentation for your platform
of choice.

### Quick Start for Developers

```bash
## Clone the edge server
git clone https://github.com/ptcrealitylab/vuforia-spatial-edge-server/

## Clone the core add-on
mkdir addons
cd addons
git clone https://github.com/ptcrealitylab/vuforia-spatial-core-addon/

## Install the dependencies in core-addon and server
cd addons/vuforia-spatial-core-addon; npm install
cd ../../; npm install

## Run
node index.js
```

To learn how to configure this server, read the [Web Interface Manager instructions](./web-interface-manager).

### Mac

Follow [the platform specific install instructions](./install-on-mac).

### Windows

An in-depth walkthrough is coming soon. The Mac instructions are mostly applicable if you want to use
GitHub Desktop and Powershell. Alternatively, check out the quick start for
developers above.

### Linux

An in-depth walkthrough is coming soon. Until then, you can follow the quick
start for developers above in your terminal of choice.

## Next

* [Learn how to use the Vuforia Spatial Toolbox](https://spatialtoolbox.vuforia.com/docs/use)
* [Learn how to add a new Object to your server](https://spatialtoolbox.vuforia.com/docs/use/connect-to-the-physical-world/create-object)
