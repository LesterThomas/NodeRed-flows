Directions using Googlemapsutil
===============================

This is a test flow to test the Googlemaps API using the googlemapsutil wrapper node (https://www.npmjs.com/package/googlemapsutil). It takes two addresses and uses the google API to calculate the distance (in km and time).

Install the googlemapsutil module:

```
	npm install googlemapsutil
```

Add a reference to this module in node-red when it starts by adding the following code to the `settings.js` file in node-red (or in `thethingbox.js` if you are using node-red on a Raspberry Pi using http://thethingbox.io/)

```javascript
    functionGlobalContext: {
    gmaputil:require('googlemapsutil')
    }
```

Finally, put the `.json` file into the `/lib/flows` folder and import the flow using the Node-Red import menu.



