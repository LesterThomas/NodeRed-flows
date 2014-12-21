This flow polls the BT router in my house to look for the mac address of connected devices. I maintain a JSON of devices I'm interested in (my mobile phones). It then logs into a Cloudant database when one of the devices registers or un-registers with the router. For registrations, it also plays a Welcome message over my Sonos. 

This flow requires the sonos Node Module https://www.npmjs.com/package/sonos

Install the sonos module:

```
	npm install sonos
```

Add a reference to this module in node-red when it starts by adding the following code to the `settings.js` file in node-red (or in `thethingbox.js` if you are using node-red on a Raspberry Pi using http://thethingbox.io/)

```javascript
    functionGlobalContext: {
        sonos:require('sonos'),
    }
```

Finally, put the `.json` file into the `/lib/flows` folder and import the flow using the Node-Red import menu.


