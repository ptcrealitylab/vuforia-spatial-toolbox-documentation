---
layout: doc
title: Starting the Vuforia Spatial Edge Server on Mac
permalink: /docs/use/connect-to-the-physical-world/install-on-mac
---

## Starting the Vuforia Spatial Edge Server on Mac

## Install Node

Follow the instructions on Node's website to install `node` and `npm`:
https://nodejs.org/en/. If installing succeeding, you'll be able to open
Terminal and run `node --version`:

![Terminal with the command node --version run, it outputs v13.5.0 without errors](./images/terminal-node-version.png)

## Get the Edge Server

### Install GitHub Desktop (Optional)

Follow the [instructions on GitHub's website](https://help.github.com/en/desktop/getting-started-with-github-desktop/installing-github-desktop) to install the GitHub desktop app. Open it up and sign in to get started.

### Download Edge Server

#### With GitHub Desktop
In GitHub Desktop select Clone Repository (under the "Add" menu if the button isn't immediately visible). Once there, paste the edge server's repository url, https://github.com/ptcrealitylab/vuforia-spatial-edge-server/ and click Clone.

![GitHub desktop with Clone menu open and edge server url filled in](./images/gh-desktop-edge-server-clone.png)

#### With a Browser
Download the current source code from [this
link](https://github.com/ptcrealitylab/vuforia-spatial-edge-server/archive/master.zip)
and unzip it. In the following example I've unzipped it then moved it into my
Documents directory:

![Finder window with edge server zip downloaded and unzipped](finder-downloads-edge-server-app.png)
![Finder window with edge server folder moved to Documents](finder-downloads-edge-server-moved.png)

### Download Core Add-on

#### With GitHub Desktop
Open GitHub Desktop and use the same Clone Repository dialog to clone
https://github.com/ptcrealitylab/vuforia-spatial-core-addon/ to the addons
subdirectory of vuforia-spatial-edge-server. Note that in the "Choose Folder"
dialog you will have to create a new folder named "addons."

![Clone dialog highlighting choose folder button](./images/gh-desktop-core-addon-choose-folder.png)
![Choose folder dialog with spatial edge server selected](./images/gh-desktop-core-addon-choose-folder-popup.png)
![Choose folder dialog with addons subfolder created](./images/gh-desktop-core-addon-choose-folder-addons-created.png)
![Clone dialog with correct folder chosen](./images/gh-desktop-core-addon-folder-chosen.png)

#### With a Browser

Download the current source code from [this
link](https://github.com/ptcrealitylab/vuforia-spatial-core-addon/archive/master.zip).
Unzip it and move it into a new folder named "addons" inside your vuforia-spatial-edge-server directory.

![Finder window with core add-ons zip downloaded and unzipped](./images/finder-downloads-addon-zip.png)
![Finder window with new folder selected in edge server folder](./images/finder-edge-server-create-addons.png)


Once you're done with all the unzipping and directory management, your full
layout should look like this:
![Finder window with full directory layout](./images/finder-full-directory-layout.png)

## Install Dependencies

Track down the Edge Server directory from the previous step in Terminal. If you
put the edge server in your Documents directory and kept the default name
`vuforia-spatial-edge-server`, you can get to it with the command `cd
~/Documents/vuforia-spatial-edge-server`.

Once you're in the correct directory, you can confirm this with the command
`ls` which should output something close to the following:

![Terminal output with cd to the edge server directory followed by ls](./images/terminal-gh-edge-server-ls.png)

Now that you've confirmed that you're in the right location you can run the
command `npm install`.

![Terminal containing npm install in the edge server directory](./images/terminal-gh-edge-server-npm-install.png)

We also need to install the dependencies for the core add-on. In the edge
server directory, run `cd addons/vuforia-spatial-core-addon` to get into the
core add-on's main directory. Here you can run `npm install` again to install
the core add-on's specific dependencies.

![Terminal containing cd into addons directory then npm install](./images/terminal-gh-edge-server-addon-install.png)

## Start the Edge Server

Now, return to the edge server's directory by running `cd ../../`. You can
confirm you're in the correct directory by running `ls` again. Now you're here,
you can run `node index.js` to start the server.

![Terminal showing returning to the edge server directory and running node](./images/terminal-gh-edge-server-start-server.png)

