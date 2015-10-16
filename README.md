#Description
This module is designed as an addon component to isomorphic-react-base-app.
It provides access to a local authentication scheme.

#Developing

Edit src folder only which is in ES6 and before deployment run 
    
    npm run-script build
    
to transcribe from ES6 to ES5 using babel. The transcribed code is placed in lib folder which is what is retrieved by npm install.

#Usage
    
    npm install ../isomorphic-react-authentication/

Register the Authentication Service in server.js

	fetchrPlugin.registerService(require('isomorphic-react-authentication').AuthenticationService);	
	
For a custom authentication function the following is needed in server.js
    
    var jwt = require('jsonwebtoken');
    var AuthenticationService = require('isomorphic-react-authentication').AuthenticationService;
    var authFunc = function(params,callback){
        var key = 'private';
        if (params.username === "spanias" && params.password === "itworks") {
            console.log("AuthenticationService: Authentication Successful!");
            var token = jwt.sign({
                user: 'spanias',
                group: 'administrator',
                email: 'demetris@spanias.com',
                verified: false
            }, key);
            callback(null, token);
        }
        else {
            console.log("AuthenticationService: Authentication Failed!");
            var err = {errorID: 1, message: 'Authentication Failed'};
            callback(err, null)
        }
    };
    AuthenticationService.setAuthenticateMethod(authFunc);
    
    fetchrPlugin.registerService(AuthenticationService);


Use the store in app.js

    var AuthenticationStore = require('isomorphic-react-authentication').AuthenticationStore;

And add it to the app's stores

        stores: [ AuthenticationStore ] 
    

In the page where the component will be rendered add the following:

	var AuthenticationComponent = require('isomorphic-react-authentication').AuthenticationComponent;

and within the render jsx 

	<AuthenticationComponent />

#Customization

Add the following in your custom .less file

    // Authentication Component properties
    .loginError {
      font-size: 12px;
      font-style: oblique;
      color: red;
    }
    .loginNormal {
      font-size: 12px;
      font-style: oblique;
      color: darkgray;
    }
    .login-modal-title {
      font-size: 22px;
      text-align: center;
      padding-top: 5px;
      padding-bottom: 5px;
    }
    .login-modal-header {
      padding-right: 15px;
    }