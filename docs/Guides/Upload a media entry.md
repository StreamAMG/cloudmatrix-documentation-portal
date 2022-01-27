# Upload a Media Entry

A client can use the CloudMatrix Upload API to upload media entries and enriched metadata into the CloudMatrix and MediaPlatform platforms.

Please refer to the API Reference documentation for Technical Implementation details.

The following sequence diagram outlines how a simple integration would work with files less than 5GB. 

![CloudMatrix Enrichment API - Phase 1 - Sequence (1).png](https://stoplight.io/api/v1/projects/cHJqOjc2ODM3/images/OiCWC6k7vtU)

For more complex/larger objects you must perform a multipart upload of content. 

The following sequence outlines how this can be achieved using the multipart API endpoints. 

![CloudMatrix Enrichment API - Phase 2 - Sequence (1).png](https://stoplight.io/api/v1/projects/cHJqOjc2ODM3/images/pROnd22MePM)

