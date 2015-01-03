CouchDb on Raspberry Pi
=======================

This shows details on installing and running CouchDb on a Raspberry Pi (running theThingBox and Node-Red).

Installation is simple:

```
apt-get install couchdb
```

This installs couchdb with default settings.

The next step is to edit the /etc/couchdb/local.ini and default.ini files. Change bind_address to current ip of your pi (without this, you can only access the database locally). I also changed the location of the database to an external network mount.

To expose the database to my application, I use a simple database proxy flow that takes a querystring parameter e.g. `http://thethingbox:1880/api/home_automation_db?querystring=_all_docs`. In my setup, I have 2x Raspberry Pi's (theThingBox, theDbBox): I run all front-end web interfaces on theThingBox (including the db Proxy), and all back-end flows and the database on theDbBox.

