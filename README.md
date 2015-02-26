Client
========
```javascript
var services = require("stremio-services");
var stremio = new services.Client({ /* options; picker: function(services) { return services } */ });
// specify a picker function to filter / sort the services we'lll use

stremio.setAuth(url, authKey); // Set the authentication for services that require auth
// URL is the URL to the central authentication server - some services only permit certain servers
// authKey is the authentication token

stremio.addService(URL, { priority: 0 }); // Priority is an integer, zero is the highest priority

stremio.meta.get(args,cb); /* OR */ stremio.call("meta.get", args, cb);
```


Server
=======
```javascript
var services = require("stremio-services");
new services.Server({
	"meta.get": function(args, cb) {
		// this.user -> get info about the user
	},
}, { allow: ["api.linvo.me"], secret: "SOME SECRET" });

```

Secret & authToken
==============
The authToken is a session ID we use for service clients to identify the user. It's the service' job (implemented in server.js) to evaluate if we're getting requests from a logged-in users. That happens by asking the central server if that authToken is valid.

The secret is a token that is used to get the central server (regulating) to authorize on the user's session. We can also use the secret as an auth token for clients with .setAuth() in order to get a special session to get services to use each other.