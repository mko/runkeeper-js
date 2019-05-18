RunKeeper.js (`runkeeper-js`) - RunKeeper HealthGraph API Client Library for Node.js
==============================================================

[![Greenkeeper badge](https://badges.greenkeeper.io/mko/runkeeper-js.svg)](https://greenkeeper.io/)

This is an API Client and Wrapper for the [RunKeeper Health Graph API](http://developer.runkeeper.com/healthgraph) in active development and maintained on NPM. It is based on `node-runkeeper` (see Contributors for more info).

## Installation

- `runkeeper-js` is available on NPM and maintained in sync with this Github branch. Just include it in your dependencies via `package.json` and `npm install` in your app's root directory to install it in your `node_modules` directory.

## Creating a client

Register your application with RunKeeper to get the credentials needed for your client options: [http://runkeeper.com/partner/applications](http://runkeeper.com/partner/applications)

```javascript

// Set up your client's options
var options = exports.options = {

    // Client ID (Required): 
    // This value is the OAuth 2.0 client ID for your application.  
    client_id : "< client id >",

    // Client Secret (Required):  
    // This value is the OAuth 2.0 shared secret for your application.   
    client_secret : "< client secret >",
    
    // Authorization URL (Optional, default will work for most apps):
    // This is the URL to which your application should redirect the user in order to authorize access to his or her RunKeeper account.   
    auth_url : "https://runkeeper.com/apps/authorize",

    // Access Token URL (Optional, default will work for most apps):
    // This is the URL at which your application can convert an authorization code to an access token. 
    access_token_url : "https://runkeeper.com/apps/token",

    // Redirect URI (Optional but defaults to null, which means your app won't be able to use the getNewToken method):
    // This is the URL that RK sends user to after successful auth  
    // URI naming based on Runkeeper convention 
    redirect_uri : "< redirect uri >",
    
    // Access Token (Optional, defaults to null):
    // When doing Client API Calls on behalf of a specific user (and not getting a new Access Token for the first time), set the user's Access Token here.
    access_token : "< access token >",

    // API Domain (Optional, default will work for most apps):
    // This is the FQDN (Fully qualified domain name) that is used in making API calls
    api_domain : "api.runkeeper.com"
};

// Require RunKeeper.js
var runkeeper = require('runkeeper-js');

// Create a Client
var client = new runkeeper.HealthGraph(options);

```

## Using the client

Using any of the client's API methods assumes that it has a valid access_token except for the getNewToken method. In order to perform API calls on behalf of a user, you first need to get their access token

```javascript
// Get a new Access Token with your Client
client.getNewToken(authorization_code, function(err, access_token) {

	// If an error occurred during the Access Token request, handle it. For the example, we'll just output it and return false.
	if(err) { console.log(err); return false; }

	// Set the client's Access Token. Any future API Calls will be performed using the authorized user's access token. 
	client.access_token = access_token;
	
	// Usually, you'll want to store the access_token for later use so that you can set it upon initialization of the Client
	
	// Example: Get user's Profile information
	client.profile(function(err, reply) {
		if(err) { console.log(err); }
		
		// Do whatever you need with the API's reply.
		console.log(reply);
	});
})
```

## Extending the client

Several Predefined API Calls are available to the Client in addition to the Generic API Call function; these are defined in the API object, as seen below.

```javascript
var API = exports.API = {
    "user": {"media_type": "application/vnd.com.runkeeper.User+json",
             "uri": "/user"},
    "profile": {"media_type": "application/vnd.com.runkeeper.Profile+json",
                "uri": "/profile"},
    "settings": {"media_type": "application/vnd.com.runkeeper.Settings+json",
                "uri": "/settings"},
    "fitnessActivityFeed": {"media_type": "application/vnd.com.runkeeper.FitnessActivityFeed+json",
                "uri": "/fitnessActivities"},
    "fitnessActivities": {"media_type": "application/vnd.com.runkeeper.FitnessActivity+json",
                "uri": "/fitnessActivities"},
};
```

## Contributors

- [node-runkeeper](https://github.com/marksoper/node-runkeeper) originally authored by [Mark Soper](https://github.com/marksoper/)
- which was then forked by [Christine Yen](https://github.com/christineyen)
- and then resurrected by [Michael Owens](https://github.com/mowens) as RunKeeper.js (`runkeeper-js` on NPM)

## License

(The MIT License)

Copyright (c) 2012 Michael Owens

Permission is hereby granted, free of charge, to any person obtaining a copy of
this software and associated documentation files (the "Software"), to deal in
the Software without restriction, including without limitation the rights to
use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of
the Software, and to permit persons to whom the Software is furnished to do so,
subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS
FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR
COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER
IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN
CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.