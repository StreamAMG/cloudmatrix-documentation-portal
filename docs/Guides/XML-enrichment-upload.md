---
stoplight-id: 1fyfja4optbcw
tags: [Upload, Cloud Matrix]
---

# XML enrichment upload

The XML enrichment upload service allows clients to add metadata to videos they are uploading to Cloud Matrix via an XML file.

## Setup
Each client will have their own S3 bucket bucket provisioned with folders to drop video `/video` and `/xml` files.  In addition to these folders there will be a `/result` folder where the service posts log files after each upload attempt (more details below).

StreamAMG will provide the following access details to the integrator:
- S3 bucket name
- Access key ID
- Secret access key

The metadata attributes and values need to be created in Cloud Matrix via the admin backend for us to successfully map the XML values.  When an attribute is created in Cloud Matrix it is assigned its own Universal Unique Identifier (UUID) which is used for the mapping.

These UUIDs are fixed and should be included in the XML as an attribute to the XML element:
```
<title cm-attribute-id="1e3547ba-784f-6283-b7817-f30s55m08788">Title of video</title>
```
There are two attribute ids we ask you to use:
1. **cm-attribute-id** where you are sending a single values such as 'title'
2. **cm-collection-id** where you are sending multiple value such as 'teams'
    - **cm-collection-value** This is used in conjunction with cm-collection-id for multiple values

#### Sample XML

```xml
<?xml version='1.0' encoding='utf-8'?>
<file-information>
  <filename>New Test.mp4</filename>
</file-information>
<cms-information>
  <title cm-attribute-id="1e3547ba-784f-6283-b7815-f30s55m08788">Title of video</title>
  <description cm-attribute-id="11af0dq4-12bf-4243-86ab-6532b6cdf56a">Description content here</description>
  <content-type cm-attribute-id="cd3171bq-90f7-4d9d-a8b1-bc06a7441cc2">Highlights</content-type>
  <people cm-collection-id="716a26e4-204c-49c1-85f6-9ab652ec9893">
    <person cm-collection-value="person 1"></person>
    <person cm-collection-value="person 2"></person>
  </people>
</cms-information>
```
<!-- theme: info -->
> #### cm-collection-value
> 
> The actual values to be sent via the cm-collection-value attribute should be sent as part of the attribute rather than the element (see sample above).

<!-- theme: warning -->
> #### The Importance of the filename value in the XML
>
> The filename value in the XML must match the title of the video file for us to associate the two files.  If they do not match the video upload will fail.

## Upload logs
After each upload attempt the service posts a text file to the `/result` folder in the clients S3 bucket.
This log file will detail if the the file was uploaded sucessfully or failed.  
```
"status":200,"messageFromEnrichmentApi":"Successfully uploaded"
```

In addition to this it logs failed metadata entries such as:

```
failed":[{"id":"fee1a27s-a9ee-4e89-a89g-94a2e3abb75f","reason":"Invalid value for attribute type specified"}
```


