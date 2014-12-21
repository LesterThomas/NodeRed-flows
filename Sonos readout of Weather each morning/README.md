This flow reads the local weather out over my Sonos at 7am on weekdays and 8am on weekends. It uses BBC Weather for the local forecast and then a text-to-speech api to generate a url to send to Sonos.

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

This flow is based on a flow by paulmooibroek http://flows.nodered.org/flow/f38309df456a0aa4c05b.

