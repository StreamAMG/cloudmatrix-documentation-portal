# Upload a Media Entry

A client can use the CloudMatrix Upload API to upload media entries into their CloudMatrix and MediaPlatform instances.

This allows clients to build rich integrations with their StreamAMG's products.

> In order to secure the API, the client must provide all server IPs they wish to use to access the upload API, in return they will be sent an API key which must be set as a Request header across all API requests. 

Please refer to the API Reference documentation for Technical Implementation details found in the left hand menu.

The following sequence diagram outlines how a simple integration would work with files less than 5GB. 

<img src="https://stoplight.io/api/v1/projects/cHJqOjc2ODM3/images/OiCWC6k7vtU" alt="CloudMatrix Enrichment API - Phase 1 - Sequence (1).png" width="900" style="align:center"/>


The following script demonstrates how your service can upload your binary file to the binary store once you have request a signed URL from the Upload API.

```javascript
const request = require('request');
const fs = require('fs');

request({
    method: 'PUT',
    uri: "https://streamamg.s3.eu-west-2.amazonaws.com/example-1.mp4?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAJJWZ7B6WCRGMKFGQ%2F20180210%2Feu-west-2%2Fs3%2Faws4_request&X-Amz-Date=20180210T171315Z&X-Amz-Expires=1800&X-Amz-Signature=12b74b0788aa036bc7c3d03b3f20c61f1f91cc9ad8873e3314255dc479a25351&X-Amz-SignedHeaders=host",
    body: fs.readFileSync('/streamamg/mock/example-1.mp4'),
    headers: {
      'Content-Type': 'video/mp4'
    }
  },
  function(error, response, body) {
      console.log('upload successful:', body);
    }
});
```

## Multipart Uploads


For more complex/larger objects (>5GB) you must perform a multipart upload of content. 

The following sequence outlines how this can be achieved using the multipart API endpoints. 

<img src="https://stoplight.io/api/v1/projects/cHJqOjc2ODM3/images/pROnd22MePM" alt="CloudMatrix Enrichment API - Phase 2 - Sequence (1).png" width="900" style="align:center"/>


