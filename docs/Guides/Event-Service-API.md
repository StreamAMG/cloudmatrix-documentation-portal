---
internal: true
---

# Event Service API (Internal)

This is an internal API for the management of CloudMatrix Events providing CRUD operations on CloudMatrix Events.

CloudMatrix Events UI will use this API to create, read, update and delete CloudMatrix Events to allow users to manage 
their events. 

An example of an event are fixtures such as matches, competitions, or press releases.

>Note that the 'delete' operation is a 'soft' delete and will not remove the CloudMatrix Event from the database.

>Note that the 'update' operation is via a 'PUT' request ('PATCH' is not supported).

## Base URL
The endpoints will be available at the following URL:

```https://api.event-service.streamamg.com/v1/```

## API Specification

The API specification is available at [Event-Service API](../../reference/Events-Service-API.yaml)