---
title: Getting Started
layout: lesson
---

# Getting Started

## Installing Pumlhorse

The first step to using Pumlhorse is installing it. If you haven't already, download and install 
a recent version of [Node.js](http://nodejs.org/). Pumlhorse is built on top of the Node platform.

Once you've installed Node, go to your command line and type `npm install -g pumlhorse`. This will install
Pumlhorse to your machine. The `-g` switch means that it will install **g**lobally. If you leave off the `-g`,
it will install to the current folder.

## Editing Pumlhorse files

Pumlhorse uses plaintext files, so you can use whatever editor you want. However, since they are written
as [YAML](http://yaml.org/), some tools may be better suited. One option is the free [Visual Studio Code editor](http://code.visualstudio.com/).

### Pumlhorse with VS Code

Code allows you to specify languages for unknown filetypes. In this case, you can specify `.puml` files to be
shown as YAML. To do so, go to `File > Preferences > User Settings` and add the following to the right side:

```json
"files.associations": {
    "*.puml": "yaml"
}
```

You can easily call Pumlhorse from inside Code via the integrated terminal.