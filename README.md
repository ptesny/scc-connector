# SCC-CONNECTOR

A package to be used as express middleware to make a connection to the connectivity service of the Cloud Foundry SCP with the onpremise SAP backend system.
Axios is used as the http client which can be used in other routes with: **req.axios**

## Documentation

```
$ npm install --save scc-connector
```

## Usage

Before your own api routes insert the middleware **route.use(sccConnector.setup)**
This ensures that:
- a oauth request is made to the connectivity service
- an access token is being returned
- global defaults are being set for axios

> In your app you MUST set the env variable  **SAP_SCC_VIRTUAL_HOSTS: '["<your-scc-virtual-host:port>"]'**
> in the manifest.yml file
> Only the first virtual host will be recognized

### Example
```
const sccConnector = require('scc-connector');

router.use(sccConnector.setup);

router.get("/ping", function(req, res) {
    req.axios.get( '/sap/bc/ping' )
      .then(response => {
         console.log(response);
         res.send(response.data);
        })
      .catch(error => {
         console.log(error);
      });  
});

```

