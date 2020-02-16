# Folder Structure

<img align="right" width="150" height="400" src="folder.svg">
sdasdsajlkdhaslkdjhdsflkajsdfhlekjfh
<br clear="right"/>

# Create new Tools

## API Reference

### Initialize the Javacript Library
Include the open hybrid functionality by instancing as following in your Java Script code:

```var toolbox = new Toolbox();```

### .addReadListener([IO Point], callback)
Every communication with the object is happening passively. This means that the object is not pushing data. If a user interface wants to read data from a Hybrid Object it first needs to send a read request.

Example:
```javascript
obj.addReadListener("led", function(e){
  input = e*255;
});
```


## Create new Interfaces

### API Reference


## Create new Nodes

## Create new Blocks

