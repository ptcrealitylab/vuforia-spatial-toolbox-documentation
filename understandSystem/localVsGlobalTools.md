## Different Options For Building Tools

There are a two distinct ways to create custom AR content and add it to the system.

1. Create a custom-designed **Local Tool** for a specific object, which will only ever have one
   instance (it appears when you look at that object) and has some special capabilities.
2. Create a **Global Tool** as an addon of an Edge Server, which will appear in the pocket menu
   of apps connected to the same WiFi network as that server, and which you can add multiple
   instances of to your scene.

*Note:  Tools have previously been known as Frames. Any references to Frames or Tools in other
code or documentation are referring to the same thing.*

### Local Tools

A **local** tool is an AR interface for a specific object. A tool with a specific name can be
created for a specific object through the web dashboard, which will create the
`realityobjects/[object-name]/[local-tool-name]/index.html` directories and default HTML contents.
This piece of AR content will appear when a client looks at the object. Local tools cannot be
deleted by clients. They can only be deleted through the web dashboard. They also cannot be moved
by clients from one object to another; they are tied to the object they were written for. Local
tools can be accessed by hardware interface APIs, for instance to connect nodes of a tool to a
stream of data from connected hardware.

> To create a new **local** tool, you need to click on the "Add Frame" button on the web
dashboard, and edit the default files that get generated in your realityobjects directory.

### Global Tools

A **global** tool is a definition of an AR interface that can be added to the scene multiple
times. Global tools exist in a tools directory of an addon, e.g.
`vuforia-spatial-edge-server/addons/[addon name]/tools/[global-frame-name]/index.html`. This
piece of AR content will appear in the client's pocket menu, if the client is connected to the
edge server which contains the global tool. If the edge server disconnects, the tool will no
longer appear in the client's pocket. The client can add as many copies of the tool to the scene
as they want. Clients can delete global tools or move them from one object to another by dragging
and dropping them.

The set of global tools that a server has can be viewed by clicking on the "Spatial Tools" button
on the web dashboard. Individual tools can be toggled on or off. If a tool is toggled off,
clients will not be able to see it in their pocket.

> To create a new **global** tool, you need to add a new addon to an edge server, and create a
directory for the tool within that.

#### Private Global Tools

There's one final, small distinction about global tools that might cause some confusion. You can
largely ignore this section and focus your attention on the difference between local vs global
tools.

However, if you want a complete understanding of the system, you should understand that tools
that are compiled into the edge server on your mobile app are considered "**private**" to that
device.

A private global tool is a global tool that runs on the local edge server that was compiled into
the app. Private global tools can only be added to the local world object. Private global tools
can also only be linked to other private global tools. If a public global tool with the same name
as a private global tool is found, the client will show the public version of the tool in the
pocket instead. You can think of private global tools as backups that can be used if a client is
alone it a network and fails to connect to any other edge servers.

> To create a new **private** global tool, you need to add an addon to the
bin/data/vuforia-spatial-edge-server within the mobile app directory, and recompile the app.

A non-private (**public**) global tool is a global tool that runs on any edge server outside of
the client. Global tools are considered non-private by default. Clients will discover all these
tools from all the edge servers in the network. Public global tools have none of the restrictions
 of private global tools.

> To create a new **public** global tool, you need to add an addon to any edge server, and run
that edge server from your computer.
