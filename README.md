#Description
This module is designed as an addon component to isomorphic-react-base-app.
It provides access to a local authentication scheme.

#Developing

Edit src folder only which is in ES6 and before deployment run 
    
    npm run-script build
    
to transcribe from ES6 to ES5 using babel. The transcribed code is placed in lib folder which is what is retrieved by npm install.

#Usage
   
    npm install ../isomorphic-react-authentication-server/

Register the Authentication Service in server.js

	fetchrPlugin.registerService(require('isomorphic-react-authentication').AuthenticationService);	
	
For a custom data connections the following is needed in server.js

    var AuthenticationService = require('isomorphic-react-authentication').AuthenticationService;
    var AWSDynamoDBConnector = require ('./app/modules/authenticationModuleServer/index').AWSDynamoDBConnector;
    
    var readonly_dynamocredentials = require('./dynamodbuserreadonly.json');
    var full_dynamocredentials = require('./dynamodbuser.json');
    var readonly_connector = new AWSDynamoDBConnector(readonly_dynamocredentials, true);
    var full_connector = new AWSDynamoDBConnector(full_dynamocredentials, false);
    
    AuthenticationService.setDataConnectors(full_connector, readonly_connector);
    AuthenticationService.setTokenPrivateKey("hiuhasidIUAHIUHiuhEIURHIiubiBFIBIaisuIUAS89219£@!!");
    AuthenticationService.setTokenExpiryPeriod(30);
    
    fetchrPlugin.registerService(AuthenticationService);

This connects an AWSDynamoDBConnector to the Authentication Service.

Credentials are provided in the files dynamodbuserreadonly.json and dynamodbuser.json

	dynamodbuserreadonly.json contains { "accessKeyId": "readonlyaccessid", "secretAccessKey": "key", "region": "eu-west-1" }
	dynamodbuser.json contains { "accessKeyId": "fullaccessid", "secretAccessKey": "otherkey", "region": "eu-west-1" }

We need two credentials one with full access and the other one with readonly access. This is done for security reasons.