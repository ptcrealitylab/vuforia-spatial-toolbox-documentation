---
layout: doc
title: Starting the Vuforia Spatial Edge Server on Windows
permalink: /docs/use/connect-to-the-physical-world/install-on-windows
---

## Starting the Vuforia Spatial Edge Server on Windows

## Install Node

Follow the instructions on Node's website to install `node` and `npm`:
[https://nodejs.org/en/](https://nodejs.org/en/). Make sure to click the
checkbox to install Chocolatey during the setup process. If installation
succeeded, you'll be able to open Command Prompt and run `node --version`:

![Terminal with the command node --version run, it outputs v12.16.2 without errors](./images/command-prompt-node-version.png)

## Get the Edge Server

### Install GitHub Desktop (Optional)

Follow the [instructions on GitHub's
website](https://help.github.com/en/desktop/getting-started-with-github-desktop/installing-github-desktop)
to install the GitHub desktop app. Open it up and sign in to get started.

### Download Edge Server

#### With GitHub Desktop
In GitHub Desktop select Clone Repository (under the "Add" menu if the button
isn't immediately visible). Once there, paste the edge server's repository url,
[https://github.com/ptcrealitylab/vuforia-spatial-edge-server/](https://github.com/ptcrealitylab/vuforia-spatial-edge-server/) and click Clone.

![GitHub desktop with Clone menu open and edge server url filled in](./images/gh-desktop-edge-server-clone.png)

#### With a Browser
Download the current source code from [this
link](https://github.com/ptcrealitylab/vuforia-spatial-edge-server/archive/master.zip)
and unzip it. In the following example I've unzipped it then moved it into my
Documents directory:

![Windows extract here dialog](./images/windows-extract-here.png)
![Windows with edge server folder extracted into Documents](./images/windows-extracted-directory.png)

## Install Dependencies

Track down the Edge Server directory from the previous step in Command Prompt
or PowerShell. If you put the edge server in your Documents directory and kept
the default name `vuforia-spatial-edge-server`, you can get to it with the
command `cd C:\Users\{your username}\Documents\vuforia-spatial-edge-server`.

Once you're in the correct directory, you can confirm this with the command
`dir` which should output something close to the following:

![Command Prompt output with cd to the edge server directory followed by dir](./images/command-prompt-edge-server-ls.png)

Now that you've confirmed that you're in the right location you can run the
command `npm install`.

We also need to install the dependencies for the core add-on. In the edge
server directory, run `cd addons/vuforia-spatial-core-addon` to get into the
core add-on's main directory. Here you can run `npm install` again to install
the core add-on's specific dependencies.

![Command Prompt containing cd into addons directory then npm install](./images/command-prompt-edge-server-addon-install.png)

## Start the Edge Server

Now, return to the edge server's directory by running `cd ../../`. You can
confirm you're in the correct directory by running `dir` again. Now you're here,
you can run `npm start` to start the server.

![Command Prompt showing returning to the edge server directory and running node](./images/command-prompt-start-server.png)

