# Upload a Media Entry

A client can use the CloudMatrix Upload API to upload media entries and enriched metadata into their CloudMatrix and MediaPlatform instances.

This allows clients to build rich integrations with their StreamAMG's products.

> In order to secure the API, the client must provide all server IPs they wish to use to access the upload API, in return they will be sent an API key which must be set as a Request header across all API requests. 

Please refer to the API Reference documentation for Technical Implementation details found in the left hand menu.

The following sequence diagram outlines how a simple integration would work with files less than 5GB. 

![CloudMatrix Enrichment API - Phase 1 - Sequence (1).png](https://stoplight.io/api/v1/projects/cHJqOjc2ODM3/images/OiCWC6k7vtU)


The following script demonstrates how your service can upload your binary file to the binary store.

```javascript
var request = require('request');
var fs = require('fs');
request({
  method: 'PUT',
  uri: "https://steamamg.s3.us-west-2.amazonaws.com/videos/file",
  body: fs.readFileSync('/streamamg/mock/example-1.mp4'),
  headers: {
    'Content-Type': 'video/mp4'
  }
},
function(error, response, body) {
  if (error) {
    console.error(error);
  } else {
    console.log('upload successful:', body);
  }
});
```

For more complex/larger objects you must perform a multipart upload of content. 

The following sequence outlines how this can be achieved using the multipart API endpoints. 

![CloudMatrix Enrichment API - Phase 2 - Sequence (1).png](https://stoplight.io/api/v1/projects/cHJqOjc2ODM3/images/pROnd22MePM)

